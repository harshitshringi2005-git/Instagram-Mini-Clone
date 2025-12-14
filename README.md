## Tech Stack 
Backend:

Node.js
Express.js
MongoDB (Mongoose)
JWT Authentication
bcrypt

Frontend:

React (Vite or CRA)
Axios
React Router


### BACKEND CODE
### Backend Setup
mkdir backend
cd backend
npm init -y
npm install express mongoose bcryptjs jsonwebtoken cors dotenv

Create server.js
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
require('dotenv').config();

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.log(err));

app.use('/api/auth', require('./routes/auth'));
app.use('/api/users', require('./routes/user'));
app.use('/api/posts', require('./routes/post'));

app.listen(5000, () => console.log('Server running on port 5000'));

Create .env
MONGO_URI=your_mongodb_url
JWT_SECRET=secret123



### Models

models/User.js

const mongoose = require('mongoose');
const UserSchema = new mongoose.Schema({
  username: String,
  email: String,
  password: String,
  followers: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  following: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }]
});
module.exports = mongoose.model('User', UserSchema);

models/Post.js
const mongoose = require('mongoose');

const PostSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  image: String,
  caption: String,
  likes: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  comments: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    text: String
  }]
}, { timestamps: true });

module.exports = mongoose.model('Post', PostSchema);


### Authentication Routes

routes/auth.js

const router = require('express').Router();
const User = require('../models/User');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');

router.post('/signup', async (req, res) => {
  const hash = await bcrypt.hash(req.body.password, 10);
  const user = new User({ ...req.body, password: hash });
  await user.save();
  res.json({ message: 'User registered' });
});

router.post('/login', async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  if (!user) return res.status(400).json({ msg: 'User not found' });

  const match = await bcrypt.compare(req.body.password, user.password);
  if (!match) return res.status(400).json({ msg: 'Invalid credentials' });

  const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET);
  res.json({ token, user });
});

module.exports = router;

### Middleware

middleware/auth.js

const jwt = require('jsonwebtoken');

module.exports = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ msg: 'No token' });

  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  req.user = decoded.id;
  next();
};


### Follow System

routes/user.js

const router = require('express').Router();
const User = require('../models/User');
const auth = require('../middleware/auth');

router.post('/follow/:id', auth, async (req, res) => {
  await User.findByIdAndUpdate(req.user, { $push: { following: req.params.id }});
  await User.findByIdAndUpdate(req.params.id, { $push: { followers: req.user }});
  res.json({ msg: 'Followed' });
});

router.post('/unfollow/:id', auth, async (req, res) => {
  await User.findByIdAndUpdate(req.user, { $pull: { following: req.params.id }});
  await User.findByIdAndUpdate(req.params.id, { $pull: { followers: req.user }});
  res.json({ msg: 'Unfollowed' });
});

module.exports = router;


### Posts, Likes, Comments, Feed

routes/post.js

const router = require('express').Router();
const Post = require('../models/Post');
const User = require('../models/User');
const auth = require('../middleware/auth');

router.post('/', auth, async (req, res) => {
  const post = new Post({ ...req.body, user: req.user });
  await post.save();
  res.json(post);
});

router.post('/like/:id', auth, async (req, res) => {
  await Post.findByIdAndUpdate(req.params.id, { $addToSet: { likes: req.user }});
  res.json({ msg: 'Liked' });
});

router.post('/unlike/:id', auth, async (req, res) => {
  await Post.findByIdAndUpdate(req.params.id, { $pull: { likes: req.user }});
  res.json({ msg: 'Unliked' });
});

router.post('/comment/:id', auth, async (req, res) => {
  await Post.findByIdAndUpdate(req.params.id, {
    $push: { comments: { user: req.user, text: req.body.text }}
  });
  res.json({ msg: 'Comment added' });
});

router.get('/feed', auth, async (req, res) => {
  const user = await User.findById(req.user);
  const posts = await Post.find({ user: { $in: user.following } })
    .populate('user comments.user');
  res.json(posts);
});

module.exports = router;



### FRONTEND CODE (React)

### Setup
npm create vite@latest frontend -- --template react
cd frontend
npm install axios react-router-dom


### Axios Instance

src/api.js

import axios from 'axios';

const API = axios.create({ baseURL: 'http://localhost:5000/api' });

API.interceptors.request.use(req => {
  const token = localStorage.getItem('token');
  if (token) req.headers.authorization = token;
  return req;
});

export default API;


### Login Page
import API from '../api';

function Login() {
  const login = async () => {
    const res = await API.post('/auth/login', { email, password });
    localStorage.setItem('token', res.data.token);
  };
}

### Feed Page
import { useEffect, useState } from 'react';
import API from '../api';

export default function Feed() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    API.get('/posts/feed').then(res => setPosts(res.data));
  }, []);

  return posts.map(p => (
    <div key={p._id}>
      <img src={p.image} />
      <p>{p.caption}</p>
      <p>Likes: {p.likes.length}</p>
    </div>
  ));
}

### DATABASE DESIGN (Schema Explanation)

User Collection

`_id` (ObjectId)
`username` (String)
`email` (String)
`password` (Hashed String)
`followers` (Array of User IDs) → **many-to-many**
`following` (Array of User IDs) → **many-to-many**

### Post Collection

`_id` (ObjectId)
`user` (User ID) → **one-to-many** (one user, many posts)
`image` (String URL)
`caption` (String)
`likes` (Array of User IDs) → **many-to-many**
`comments` (Array of objects)

   `user` (User ID)
   `text` (String)


## API DESIGN

### Auth APIs

| Method | Endpoint         | Description             |
| ------ | ---------------- | ----------------------- |
| POST   | /api/auth/signup | Register user           |
| POST   | /api/auth/login  | Login user & return JWT |

### User APIs

| POST | /api/users/follow/:id | Follow user |
| POST | /api/users/unfollow/:id | Unfollow user |

### Post APIs

| POST | /api/posts | Create post |
| POST | /api/posts/like/:id | Like post |
| POST | /api/posts/unlike/:id | Unlike post |
| POST | /api/posts/comment/:id | Add comment |
| GET | /api/posts/feed | Get feed posts |

## POSTMAN COLLECTION

Create a Postman collection with:

1. Signup request
2. Login request (save token in environment)
3. Create post (use token)
4. Follow / Unfollow
5. Like / Comment
6. Feed API





## README.md 

# Instagram Mini Clone Backend

## Tech Stack
Node.js
Express.js
MongoDB
JWT Authentication

## Setup Instructions

npm install
node server.js

## Environment Variables


MONGO_URI=your_mongodb_url
JWT_SECRET=your_secret


## Features

User Authentication
Follow / Unfollow
Create Posts
Like & Comment
Feed from followed users

## API Documentation

Refer to Postman collection included in repo.


# 📸 Instagram Mini Clone

> A full-stack social media application inspired by Instagram, built with **React, Node.js, Express.js, and MongoDB**, featuring JWT authentication, user follow systems, posts, likes, comments, and personalized feeds.

---

## 🚀 Project Overview

**Instagram Mini Clone** is a full-stack social media application developed to demonstrate the implementation of a modern web application's **frontend, backend, REST APIs, authentication, database relationships, and social-media features**.

The application allows users to:

* 🔐 Register and log in securely
* 👤 Manage user relationships
* ➕ Follow and unfollow users
* 📸 Create posts
* ❤️ Like and unlike posts
* 💬 Add comments
* 📰 View posts from followed users
* 🔑 Authenticate API requests using JWT

---

## ✨ Key Features

### 🔐 User Authentication

* User registration
* User login
* Password hashing using `bcryptjs`
* JWT-based authentication
* Protected API routes
* Token-based frontend authentication

### 👥 Follow System

* Follow users
* Unfollow users
* Store followers and following relationships
* Personalized feed based on followed users

### 📸 Post Management

* Create posts
* Store image URLs
* Add captions
* Associate posts with users
* Display posts in the user's feed

### ❤️ Likes

* Like posts
* Unlike posts
* Prevent duplicate likes using MongoDB `$addToSet`

### 💬 Comments

* Add comments to posts
* Associate comments with users
* Store comment text

### 📰 Personalized Feed

The feed API retrieves posts created by users that the authenticated user follows.

```text
Logged-in User
      ↓
Following List
      ↓
Find Posts
      ↓
Populate User & Comments
      ↓
Personalized Feed
```

---

## 🛠️ Tech Stack

### Backend

* **Node.js**
* **Express.js**
* **MongoDB**
* **Mongoose**
* **JWT Authentication**
* **bcryptjs**
* **CORS**
* **dotenv**

### Frontend

* **React**
* **Vite**
* **Axios**
* **React Router**

### Development Tools

* **Git & GitHub**
* **Postman**
* **VS Code**
* **MongoDB**

---

## 🏗️ Project Architecture

```text
                         ┌──────────────────────┐
                         │      React UI        │
                         │   Vite + Axios       │
                         └──────────┬───────────┘
                                    │
                                    │ REST API
                                    ▼
                         ┌──────────────────────┐
                         │    Express.js API    │
                         │      Node.js         │
                         └──────────┬───────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
                    ▼               ▼               ▼
              Authentication    User System     Post System
                    │               │               │
                    ▼               ▼               ▼
                  JWT          Follow/Unfollow   Like/Comment
                                    │               │
                                    └───────┬───────┘
                                            ▼
                                  ┌──────────────────┐
                                  │     MongoDB      │
                                  │    Mongoose      │
                                  └──────────────────┘
```

---

## 📁 Suggested Project Structure

```text
Instagram-Mini-Clone/
│
├── backend/
│   ├── middleware/
│   │   └── auth.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Post.js
│   │
│   ├── routes/
│   │   ├── auth.js
│   │   ├── user.js
│   │   └── post.js
│   │
│   ├── server.js
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── src/
│   │   ├── api.js
│   │   ├── components/
│   │   └── pages/
│   │
│   ├── package.json
│   └── ...
│
├── postman/
│   └── Instagram-Mini-Clone.postman_collection.json
│
└── README.md
```

---

# ⚙️ Backend Setup

## 1. Create Backend

```bash
mkdir backend
cd backend
npm init -y
```

## 2. Install Dependencies

```bash
npm install express mongoose bcryptjs jsonwebtoken cors dotenv
```

## 3. Start the Backend

```bash
node server.js
```

The backend runs on:

```text
http://localhost:5000
```

---

## 🔐 Environment Variables

Create a `.env` file inside the backend directory:

```env
MONGO_URI=your_mongodb_url
JWT_SECRET=your_secret_key
```

> ⚠️ Never commit your real MongoDB connection string or JWT secret to GitHub.

---

# 💻 Frontend Setup

Create the React application using Vite:

```bash
npm create vite@latest frontend -- --template react
```

Navigate to the frontend:

```bash
cd frontend
```

Install dependencies:

```bash
npm install axios react-router-dom
```

Start the development server:

```bash
npm run dev
```

---

# 🔌 API Configuration

The frontend communicates with the backend through Axios.

```javascript
const API = axios.create({
  baseURL: 'http://localhost:5000/api'
});
```

The Axios interceptor automatically attaches the authentication token to API requests.

```text
React Application
       ↓
     Axios
       ↓
JWT Token
       ↓
Express API
       ↓
MongoDB
```

---

# 🔑 Authentication Flow

```text
User Signup
     ↓
Password received
     ↓
bcrypt password hashing
     ↓
User stored in MongoDB
```

For login:

```text
User Login
     ↓
Check email
     ↓
Compare password using bcrypt
     ↓
Generate JWT
     ↓
Return token
     ↓
Store token on frontend
```

Protected requests use the JWT token for authentication.

---

# 🗄️ Database Design

## User Collection

| Field       | Type     | Description                 |
| ----------- | -------- | --------------------------- |
| `_id`       | ObjectId | Unique user ID              |
| `username`  | String   | Username                    |
| `email`     | String   | User email                  |
| `password`  | String   | Hashed password             |
| `followers` | Array    | IDs of followers            |
| `following` | Array    | IDs of users being followed |

### Relationship

```text
User
 ├── followers[]
 └── following[]
```

The follower/following system represents a **many-to-many relationship** between users.

---

## Post Collection

| Field       | Type     | Description                     |
| ----------- | -------- | ------------------------------- |
| `_id`       | ObjectId | Unique post ID                  |
| `user`      | ObjectId | Post owner                      |
| `image`     | String   | Image URL                       |
| `caption`   | String   | Post caption                    |
| `likes`     | Array    | IDs of users who liked the post |
| `comments`  | Array    | Post comments                   |
| `createdAt` | Date     | Creation timestamp              |
| `updatedAt` | Date     | Last update timestamp           |

### Relationships

```text
User
  │
  └──────< Posts
           │
           ├── Likes
           │
           └── Comments
```

One user can create multiple posts, representing a **one-to-many relationship**.

---

# 🌐 REST API Documentation

## 🔐 Authentication APIs

| Method | Endpoint           | Description           |
| ------ | ------------------ | --------------------- |
| `POST` | `/api/auth/signup` | Register a new user   |
| `POST` | `/api/auth/login`  | Login and receive JWT |

---

## 👥 User APIs

| Method | Endpoint                  | Description     |
| ------ | ------------------------- | --------------- |
| `POST` | `/api/users/follow/:id`   | Follow a user   |
| `POST` | `/api/users/unfollow/:id` | Unfollow a user |

---

## 📸 Post APIs

| Method | Endpoint                 | Description                   |
| ------ | ------------------------ | ----------------------------- |
| `POST` | `/api/posts`             | Create a post                 |
| `POST` | `/api/posts/like/:id`    | Like a post                   |
| `POST` | `/api/posts/unlike/:id`  | Unlike a post                 |
| `POST` | `/api/posts/comment/:id` | Add a comment                 |
| `GET`  | `/api/posts/feed`        | Get posts from followed users |

---

# 🧪 Postman Testing

The API can be tested using **Postman**.

Recommended request flow:

```text
1. Signup
   ↓
2. Login
   ↓
3. Store JWT Token
   ↓
4. Create Post
   ↓
5. Follow User
   ↓
6. Like Post
   ↓
7. Add Comment
   ↓
8. Get Personalized Feed
```

### Suggested Postman Collection

```text
Instagram Mini Clone
│
├── Authentication
│   ├── Signup
│   └── Login
│
├── Users
│   ├── Follow
│   └── Unfollow
│
└── Posts
    ├── Create Post
    ├── Like Post
    ├── Unlike Post
    ├── Add Comment
    └── Get Feed
```

---

# 🧠 Core Backend Logic

### Create Post

```text
Authenticated User
       ↓
JWT Verification
       ↓
Extract User ID
       ↓
Create Post
       ↓
Store in MongoDB
```

### Like Post

```text
User
 ↓
JWT Authentication
 ↓
Post ID
 ↓
$addToSet
 ↓
Like Added
```

### Unlike Post

```text
User
 ↓
JWT Authentication
 ↓
Post ID
 ↓
$pull
 ↓
Like Removed
```

### Personalized Feed

```text
Authenticated User
        ↓
Find Following List
        ↓
Find Posts
        ↓
Populate User & Comments
        ↓
Return Feed
```

---

# 📊 GitHub Stats

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=harshitshringi2005-git&show_icons=true&include_all_commits=true&count_private=true&hide_border=true" height="180"/>
  <img src="https://streak-stats.demolab.com/?user=harshitshringi2005-git&hide_border=true" height="180"/>
</p>

---

# 🎯 Learning Outcomes

This project demonstrates practical experience with:

* Full-stack application development
* REST API development
* Node.js and Express.js
* MongoDB database design
* Mongoose ODM
* JWT authentication
* Password hashing
* React frontend development
* Axios API integration
* React Router
* MongoDB relationships
* CRUD operations
* Social-media application logic
* API testing with Postman
* Git and GitHub

---

# 🔮 Future Improvements

Possible improvements include:

* 📱 Responsive Instagram-style UI
* 🖼️ Image upload using cloud storage
* 🔔 Real-time notifications
* 💬 Real-time messaging
* 🔍 User and post search
* ❤️ Like/unlike toggle on frontend
* 👤 User profile pages
* 📝 Edit and delete posts
* 🛡️ Improved API error handling
* 📄 Pagination and infinite scrolling
* ☁️ Cloud deployment
* 🐳 Docker support

---

# 👨‍💻 Author

**Harshit Shringi**

B.Tech Computer Science Engineering
Medicaps University, Indore

### Connect With Me

* 💻 GitHub: `https://github.com/harshitshringi2005-git`
* 💼 LinkedIn: `https://www.linkedin.com/in/harshitshringi`

---

⭐ If you found this project useful, consider giving the repository a **star**!

---

# 📸 Instagram Mini Clone

<p align="center">
  <b>A full-stack social media application inspired by Instagram</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Frontend-React-61DAFB?style=for-the-badge&logo=react&logoColor=black"/>
  <img src="https://img.shields.io/badge/Backend-Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white"/>
  <img src="https://img.shields.io/badge/API-Express.js-000000?style=for-the-badge&logo=express&logoColor=white"/>
  <img src="https://img.shields.io/badge/Database-MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Auth-JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white"/>
  <img src="https://img.shields.io/badge/Styling-CSS-1572B6?style=for-the-badge&logo=css3&logoColor=white"/>
  <img src="https://img.shields.io/badge/Testing-Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white"/>
  <img src="https://img.shields.io/badge/Build-Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white"/>
</p>

---

## 🌟 Overview

**Instagram Mini Clone** is a full-stack social media application built to demonstrate the development of a modern web application using **React, Node.js, Express.js, and MongoDB**.

The project implements core social-media functionality including **user authentication, follow/unfollow relationships, post creation, likes, comments, and personalized feeds**.

It also demonstrates how a React frontend communicates with a Node.js REST API and how user authentication can be handled using **JWT tokens**.

---

## ✨ Features

| Feature              | Description                           |
| -------------------- | ------------------------------------- |
| 🔐 Authentication    | User signup and login with JWT        |
| 🔒 Password Security | Password hashing with bcryptjs        |
| 👤 User System       | User accounts and relationships       |
| ➕ Follow             | Follow other users                    |
| ➖ Unfollow           | Remove following relationships        |
| 📸 Posts             | Create posts with images and captions |
| ❤️ Likes             | Like and unlike posts                 |
| 💬 Comments          | Add comments to posts                 |
| 📰 Feed              | View posts from followed users        |
| 🔑 Protected APIs    | JWT-based route protection            |
| 🔗 REST APIs         | Structured backend API architecture   |

---

# 🏗️ Application Architecture

```text
                    ┌───────────────────────┐
                    │      React Frontend   │
                    │       Vite + Axios    │
                    └───────────┬───────────┘
                                │
                                │ HTTP / REST API
                                ▼
                    ┌───────────────────────┐
                    │      Express.js       │
                    │       Node.js         │
                    └───────────┬───────────┘
                                │
               ┌────────────────┼────────────────┐
               │                │                │
               ▼                ▼                ▼
        Authentication      User System      Post System
               │                │                │
               ▼                ▼                ▼
             JWT          Follow/Unfollow    Like/Comment
                                │                │
                                └───────┬────────┘
                                        ▼
                              ┌──────────────────┐
                              │     MongoDB      │
                              │    Mongoose ODM  │
                              └──────────────────┘
```

---

# 🔐 Authentication Flow

```text
                   SIGNUP
                     │
                     ▼
              User Credentials
                     │
                     ▼
              bcryptjs Hashing
                     │
                     ▼
              MongoDB User
```

```text
                    LOGIN
                      │
                      ▼
                Find User
                      │
                      ▼
              Verify Password
                      │
                      ▼
                 Generate JWT
                      │
                      ▼
              Frontend Token
                      │
                      ▼
             Protected API Calls
```

---

# 📰 Feed Flow

The personalized feed is generated using the authenticated user's following list.

```text
Logged-in User
      │
      ▼
Following Users
      │
      ▼
Find Their Posts
      │
      ▼
Populate User & Comments
      │
      ▼
Personalized Feed
```

---

# 🛠️ Tech Stack

### Frontend

* ⚛️ React
* ⚡ Vite
* 🔗 Axios
* 🧭 React Router
* 🎨 CSS

### Backend

* 🟢 Node.js
* 🚂 Express.js
* 🍃 Mongoose
* 🔐 JSON Web Token
* 🔒 bcryptjs
* 🌐 CORS
* ⚙️ dotenv

### Database

* 🍃 MongoDB

### Development & Testing

* Git
* GitHub
* Postman
* VS Code

---

# 📁 Project Structure

```text
Instagram-Mini-Clone/
│
├── 📂 backend/
│   │
│   ├── 📂 middleware/
│   │   └── auth.js
│   │
│   ├── 📂 models/
│   │   ├── User.js
│   │   └── Post.js
│   │
│   ├── 📂 routes/
│   │   ├── auth.js
│   │   ├── user.js
│   │   └── post.js
│   │
│   ├── server.js
│   ├── package.json
│   └── .env
│
├── 📂 frontend/
│   │
│   ├── 📂 src/
│   │   ├── api.js
│   │   ├── 📂 components/
│   │   └── 📂 pages/
│   │
│   ├── package.json
│   └── ...
│
├── 📂 postman/
│   └── Instagram-Mini-Clone.postman_collection.json
│
└── README.md
```

---

# 🗄️ Database Design

## 👤 User Collection

| Field       | Type     | Description           |
| ----------- | -------- | --------------------- |
| `_id`       | ObjectId | Unique user ID        |
| `username`  | String   | Username              |
| `email`     | String   | User email            |
| `password`  | String   | Hashed password       |
| `followers` | Array    | IDs of followers      |
| `following` | Array    | IDs of followed users |

### Relationship

```text
                 ┌─────────────┐
                 │    User     │
                 └──────┬──────┘
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
        followers[]          following[]
```

The follower/following system represents a **many-to-many relationship** between users.

---

## 📸 Post Collection

| Field       | Type     | Description            |
| ----------- | -------- | ---------------------- |
| `_id`       | ObjectId | Unique post ID         |
| `user`      | ObjectId | Post owner             |
| `image`     | String   | Image URL              |
| `caption`   | String   | Post caption           |
| `likes`     | Array    | IDs of users who liked |
| `comments`  | Array    | Post comments          |
| `createdAt` | Date     | Creation time          |
| `updatedAt` | Date     | Last update time       |

### Relationship

```text
              ┌─────────────┐
              │    User     │
              └──────┬──────┘
                     │
                     │ 1 : N
                     ▼
              ┌─────────────┐
              │    Posts    │
              └──────┬──────┘
                     │
              ┌──────┴──────┐
              ▼             ▼
           Likes         Comments
```

---

# 🌐 REST API

## 🔐 Authentication

| Method | Endpoint           | Description           |
| ------ | ------------------ | --------------------- |
| `POST` | `/api/auth/signup` | Register a new user   |
| `POST` | `/api/auth/login`  | Login and receive JWT |

## 👥 Users

| Method | Endpoint                  | Description     |
| ------ | ------------------------- | --------------- |
| `POST` | `/api/users/follow/:id`   | Follow a user   |
| `POST` | `/api/users/unfollow/:id` | Unfollow a user |

## 📸 Posts

| Method | Endpoint                 | Description           |
| ------ | ------------------------ | --------------------- |
| `POST` | `/api/posts`             | Create a post         |
| `POST` | `/api/posts/like/:id`    | Like a post           |
| `POST` | `/api/posts/unlike/:id`  | Unlike a post         |
| `POST` | `/api/posts/comment/:id` | Add a comment         |
| `GET`  | `/api/posts/feed`        | Get personalized feed |

---

# ⚙️ Backend Setup

### 1. Create the backend

```bash
mkdir backend
cd backend
npm init -y
```

### 2. Install dependencies

```bash
npm install express mongoose bcryptjs jsonwebtoken cors dotenv
```

### 3. Start the server

```bash
node server.js
```

Backend:

```text
http://localhost:5000
```

---

# 💻 Frontend Setup

Create the React application:

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

Run the application:

```bash
npm run dev
```

---

# 🔑 Environment Configuration

Create `.env` inside the backend directory:

```env
MONGO_URI=your_mongodb_url
JWT_SECRET=your_secret_key
```

> ⚠️ **Never commit real API keys, database credentials, or secrets to GitHub.**

---

# 🔗 Axios Configuration

The frontend uses Axios to communicate with the backend.

```javascript
import axios from 'axios';

const API = axios.create({
  baseURL: 'http://localhost:5000/api'
});

API.interceptors.request.use(req => {
  const token = localStorage.getItem('token');

  if (token) {
    req.headers.authorization = token;
  }

  return req;
});

export default API;
```

---

# 🧪 Postman Testing

The backend REST APIs can be tested using **Postman**.

Recommended testing sequence:

```text
1️⃣ Signup
     ↓
2️⃣ Login
     ↓
3️⃣ Save JWT Token
     ↓
4️⃣ Create Post
     ↓
5️⃣ Follow User
     ↓
6️⃣ Like Post
     ↓
7️⃣ Add Comment
     ↓
8️⃣ Get Feed
```

### Suggested Postman Collection

```text
📦 Instagram Mini Clone
│
├── 🔐 Authentication
│   ├── Signup
│   └── Login
│
├── 👥 Users
│   ├── Follow
│   └── Unfollow
│
└── 📸 Posts
    ├── Create Post
    ├── Like Post
    ├── Unlike Post
    ├── Add Comment
    └── Get Feed
```

---

# 🧠 Key Technical Concepts Demonstrated

This project demonstrates practical implementation of:

```text
React
  ↓
REST APIs
  ↓
Node.js + Express
  ↓
JWT Authentication
  ↓
MongoDB + Mongoose
  ↓
Database Relationships
  ↓
Social Media Features
```

### Core Concepts

* REST API development
* Authentication & authorization
* Password hashing
* JWT token management
* Middleware
* MongoDB schema design
* Mongoose relationships
* CRUD operations
* React state management
* Axios API integration
* Protected routes
* API testing

---



# 🎯 Learning Outcomes

Through this project, I gained practical experience in:

* Building full-stack web applications
* Designing REST APIs
* Working with MongoDB and Mongoose
* Implementing JWT authentication
* Securing passwords with bcryptjs
* Connecting React with backend APIs
* Designing relational data structures in MongoDB
* Testing APIs with Postman
* Structuring a scalable backend
* Using Git and GitHub for project management

---

# 🔮 Future Improvements

* 📱 Fully responsive Instagram-style UI
* 🖼️ Cloud-based image uploads
* 👤 Complete user profile pages
* 🔍 User and post search
* 🔔 Real-time notifications
* 💬 Real-time messaging
* 📝 Edit and delete posts
* ❤️ Interactive like/unlike UI
* 📄 Pagination and infinite scrolling
* 🛡️ Improved validation and error handling
* ☁️ Cloud deployment
* 🐳 Docker containerization

---

# 👨‍💻 Author

### Harshit Shringi

**B.Tech Computer Science Engineering**
Medicaps University, Indore

<p align="left">
  <a href="https://github.com/harshitshringi2005-git">
    <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/>
  </a>
  <a href="https://www.linkedin.com/in/harshitshringi">
    <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/>
  </a>
</p>

---

## 📊 GitHub Stats

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=harshitshringi2005-git&show_icons=true&include_all_commits=true&count_private=true&hide_border=true" height="180"/>
  <img src="https://streak-stats.demolab.com/?user=harshitshringi2005-git&hide_border=true" height="180"/>
</p>

---

<p align="center">
  ⭐ <b>If you found this project useful, consider giving it a star!</b>
</p>

<p align="center">
  Built with ❤️ using React, Node.js, Express.js & MongoDB
</p>

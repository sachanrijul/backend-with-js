# VideoTube Backend 📹

VideoTube is a production-grade backend for a video hosting platform, architected with **Node.js**, **Express**, and **MongoDB**. It provides a scalable foundation for features like video streaming, user management, and social interactions.

> **Status**: Active Development 🚧

## 🌟 Key Features

### 🔐 Authentication & Security
- **JWT Authentication**: Robust access and refresh token rotation with HTTP-only cookies.
- **Password Security**: Passwords are hashed using `bcrypt` before storage.
- **Protected Routes**: Middleware (`verifyJWT`) ensures only authenticated users access sensitive endpoints.

### 📹 Video Management
- **Media Uploads**: Seamless integration with **Cloudinary** for storing videos and thumbnails.
- **Multi-step Upload**: Uses `Multer` for temporary local storage before cloud upload to ensure reliability.
- **Aggregations**: Complex MongoDB aggregation pipelines to calculate views, subscribers, and watch history efficiently.

### 👤 User & Social
- **Channel Profiles**: Detailed user profiles with subscriber counts and subscription status.
- **Watch History**: Tracks user activity for personalized experiences.
- **Subscription System**: Efficient "Many-to-Many" relationship handling for subscriptions.

---

## 📚 API References

### User Routes (`/api/v1/users`)

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/register` | Register a new user (with avatar & cover image) | ❌ |
| `POST` | `/login` | Login user and receive tokens | ❌ |
| `POST` | `/logout` | Logout user (clears cookies) | ✅ |
| `POST` | `/refresh-token` | Generate new access token using refresh token | ❌ |
| `POST` | `/change-password` | Update user password | ✅ |
| `GET` | `/current-user` | Get logged-in user details | ✅ |
| `PATCH` | `/update-account` | Update name and email | ✅ |
| `PATCH` | `/avatar` | Update profile picture | ✅ |
| `PATCH` | `/cover-image` | Update channel cover image | ✅ |
| `GET` | `/c/:username` | Get public channel profile (subscribers info) | ✅ |
| `GET` | `/history` | Get user watch history | ✅ |

*(More routes for Videos, Comments, and Tweets to be added)*

---

## 🛠️ Data Models

### **User**
- `username`, `email`, `fullname`: Standard identity fields.
- `avatar`, `coverImage`: Cloudinary URLs.
- `watchHistory`: Array of `Video` references.
- `password`: Hashed string.
- `refreshToken`: For session management.

### **Video**
- `videoFile`, `thumbnail`: Cloudinary URLs.
- `owner`: Reference to `User`.
- `title`, `description`, `duration`, `views`.
- `isPublished`: Boolean flag for visibility.

### **Subscription**
- `subscriber`: Reference to `User` (Who is following).
- `channel`: Reference to `User` (Who is being followed).

---

## ⚙️ Local Development Setup

### Prerequisites
- **Node.js** (v14+ recommended)
- **MongoDB** (Local or Atlas)
- **Cloudinary Account** (for API keys)

### Step-by-Step Guide

1.  **Clone the Repository**
    ```bash
    git clone https://github.com/your-username/backend.git
    cd backend
    ```

2.  **Install Dependencies**
    ```bash
    npm install
    ```

3.  **Configure Environment**
    Create a `.env` file in the root directory. Copy the following template:

    ```env
    PORT=8000
    MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/videotube
    CORS_ORIGIN=*
    
    # JWT Secrets (Generate random strings)
    ACCESS_TOKEN_SECRET=your_complex_access_token_secret
    ACCESS_TOKEN_EXPIRY=1d
    REFRESH_TOKEN_SECRET=your_complex_refresh_token_secret
    REFRESH_TOKEN_EXPIRY=10d

    # Cloudinary Credentials
    CLOUDINARY_CLOUD_NAME=abc123xyz
    CLOUDINARY_API_KEY=123456789
    CLOUDINARY_API_SECRET=your_api_secret_here
    ```

4.  **Run the Server**
    ```bash
    npm run dev
    ```
    You should see:
    > MongoDB connected !! DB host ...
    > Server is running at port: 8000

---

## 📂 Project Structure

```bash
src/
├── controllers/    # Logic for handling requests (e.g., user.controller.js)
├── db/             # Database connection logic
├── middlewares/    # Express middlewares (auth, multer)
├── models/         # Mongoose Data Schemas
├── routes/         # API Route definitions
├── utils/          # Utility functions (ApiResponse, ApiError, Cloudinary)
├── app.js          # Express App configuration
└── index.js        # Entry point
```

## 🤝 Contributing
1. Fork the repo.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

## 📝 License
Distributed under the ISC License.

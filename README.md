# Project Camp Backend

A comprehensive RESTful API service for collaborative project management, built with Node.js, Express, and MongoDB.

## 🚀 Features

- **User Authentication & Authorization**
  - JWT-based authentication with refresh tokens
  - Email verification
  - Password reset functionality
  - Role-based access control (Admin, Project Admin, Member)

- **Project Management**
  - Create, read, update, and delete projects
  - Team member management with role assignment
  - Project-level access control

- **Task Management**
  - Hierarchical task and subtask system
  - Task assignment to team members
  - Status tracking (Todo, In Progress, Done)
  - File attachments support

- **Project Notes**
  - Create and manage project documentation
  - Admin-only creation and modification

- **Security**
  - JWT token-based authentication
  - Role-based authorization
  - Input validation
  - Secure file uploads

## 🛠️ Tech Stack

- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB with Mongoose ODM
- **Authentication:** JWT (jsonwebtoken)
- **Validation:** express-validator
- **File Upload:** Multer
- **Email:** Nodemailer with Mailgen
- **Security:** bcrypt for password hashing

## 📋 Prerequisites

- Node.js (v14 or higher)
- MongoDB (local or Atlas)
- npm or yarn

## ⚙️ Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/projectcamp-backend.git
   cd projectcamp-backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Setup**
   
   Create a `.env` file in the root directory:
   ```env
   PORT=8000
   MONGODB_URI=your_mongodb_connection_string
   CORS_ORIGIN=*
   
   ACCESS_TOKEN_SECRET=your_access_token_secret
   ACCESS_TOKEN_EXPIRY=1d
   REFRESH_TOKEN_SECRET=your_refresh_token_secret
   REFRESH_TOKEN_EXPIRY=10d
   
   MAILTRAP_SMTP_HOST=smtp.mailtrap.io
   MAILTRAP_SMTP_PORT=2525
   MAILTRAP_SMTP_USER=your_mailtrap_user
   MAILTRAP_SMTP_PASS=your_mailtrap_password
   ```

4. **Run the application**
   ```bash
   # Development mode
   npm run dev
   
   # Production mode
   npm start
   ```

## 📚 API Documentation

### Base URL
```
http://localhost:8000/api/v1
```

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/auth/register` | Register new user | No |
| POST | `/auth/login` | Login user | No |
| POST | `/auth/logout` | Logout user | Yes |
| GET | `/auth/current-user` | Get current user | Yes |
| POST | `/auth/change-password` | Change password | Yes |
| POST | `/auth/refresh-token` | Refresh access token | No |
| GET | `/auth/verify-email/:token` | Verify email | No |
| POST | `/auth/forgot-password` | Request password reset | No |
| POST | `/auth/reset-password/:token` | Reset password | No |
| POST | `/auth/resend-email-verification` | Resend verification email | Yes |

### Project Endpoints

| Method | Endpoint | Description | Auth Required | Role |
|--------|----------|-------------|---------------|------|
| GET | `/projects` | Get all user projects | Yes | All |
| POST | `/projects` | Create new project | Yes | All |
| GET | `/projects/:projectId` | Get project details | Yes | Member+ |
| PUT | `/projects/:projectId` | Update project | Yes | Admin |
| DELETE | `/projects/:projectId` | Delete project | Yes | Admin |
| GET | `/projects/:projectId/members` | Get project members | Yes | Member+ |
| POST | `/projects/:projectId/members` | Add project member | Yes | Admin |
| PUT | `/projects/:projectId/members/:userId` | Update member role | Yes | Admin |
| DELETE | `/projects/:projectId/members/:userId` | Remove member | Yes | Admin |

### Task Endpoints

| Method | Endpoint | Description | Auth Required | Role |
|--------|----------|-------------|---------------|------|
| GET | `/tasks/:projectId` | Get project tasks | Yes | Member+ |
| POST | `/tasks/:projectId` | Create task | Yes | Admin/Project Admin |
| GET | `/tasks/:projectId/t/:taskId` | Get task details | Yes | Member+ |
| PUT | `/tasks/:projectId/t/:taskId` | Update task | Yes | Admin/Project Admin |
| DELETE | `/tasks/:projectId/t/:taskId` | Delete task | Yes | Admin/Project Admin |
| POST | `/tasks/:projectId/t/:taskId/subtasks` | Create subtask | Yes | Admin/Project Admin |
| PUT | `/tasks/:projectId/st/:subTaskId` | Update subtask | Yes | Member+ |
| DELETE | `/tasks/:projectId/st/:subTaskId` | Delete subtask | Yes | Admin/Project Admin |

### Note Endpoints

| Method | Endpoint | Description | Auth Required | Role |
|--------|----------|-------------|---------------|------|
| GET | `/notes/:projectId` | Get project notes | Yes | Member+ |
| POST | `/notes/:projectId` | Create note | Yes | Admin |
| GET | `/notes/:projectId/n/:noteId` | Get note details | Yes | Member+ |
| PUT | `/notes/:projectId/n/:noteId` | Update note | Yes | Admin |
| DELETE | `/notes/:projectId/n/:noteId` | Delete note | Yes | Admin |

### Health Check

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/healthcheck` | Check API health | No |

## 🔐 Authentication

This API uses JWT tokens for authentication. After login, you'll receive an access token and refresh token.

### Using the API

1. **Register/Login** to get tokens
2. **Include the access token** in subsequent requests:
   ```
   Authorization: Bearer <your_access_token>
   ```
3. **Refresh token** when access token expires using `/auth/refresh-token`

## 👥 User Roles & Permissions

| Role | Permissions |
|------|-------------|
| **Admin** | Full project control, manage members, create/edit/delete tasks, notes |
| **Project Admin** | Create/edit/delete tasks and subtasks |
| **Member** | View projects, tasks, notes; update subtask status |

## 📁 Project Structure

```
projectcamp-backend/
├── src/
│   ├── controllers/      # Request handlers
│   ├── middlewares/      # Custom middlewares
│   ├── models/          # Mongoose models
│   ├── routes/          # API routes
│   ├── utils/           # Utility functions
│   ├── validators/      # Input validation
│   ├── constants.js     # Application constants
│   ├── app.js          # Express app setup
│   └── index.js        # Entry point
├── public/
│   └── images/         # Uploaded files
├── .env                # Environment variables
├── .gitignore
├── package.json
└── README.md
```

## 🧪 Testing

Example registration request:
```bash
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "username": "johndoe",
    "password": "SecurePass123!"
  }'
```

Example login request:
```bash
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!"
  }'
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License.

## 👨‍💻 Author

MD AL Fahad - [@yourhandle](https://github.com/md-al-fahad)

## 🙏 Acknowledgments

- Express.js for the web framework
- MongoDB for the database
- JWT for authentication
- All other open-source libraries used in this project


---

**Note:** Remember to never commit your `.env` file or expose sensitive credentials.

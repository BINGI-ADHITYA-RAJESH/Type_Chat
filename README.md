# Chat Application Documentation
## Project Overview
### Purpose
The Chat Application is a real-time messaging platform built using Node.js, Express.js, TypeScript, and Socket.IO. It allows users to communicate with each other through text and file sharing.
### Key Features
- User authentication (signup, login, logout, refresh token)
- Real-time messaging via WebSockets
- Chat creation and listing
- File sharing
- REST API for user and chat management
- PostgreSQL database for persistent storage
- Redis for storing user tokens and online user status
- Swagger documentation for API endpoints
### Supported Platforms/Requirements
- Node.js (v16 or higher)
- PostgreSQL
- Redis
- Docker (optional, for containerization)
## Getting Started
### Installation
1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd chat_app
    ```
2.  **Install global Node.js packages:**
    ```bash
    npm install -g ts-node@10.5.0
    npm install -g nodemon
    npm install -g db-migrate
    npm install -g db-migrate-pg
    ```
3.  **Install dependencies:**
    ```bash
    npm install
    ```
4.  **Configure environment variables:**
    - Create a `.env` file based on `.env.sample`.
    - Fill in the required values for database connection, Redis, JWT secrets, etc.
        ENV=dev
    HOST=localhost
    PORT=8050
    DATABASE_URL=postgres://USER:PASSWORD@localhost:5442/chat_app
    PG_USERNAME=postgres
    PG_PASSWORD=pass123
    PG_DATABASE=chat_app
    PG_HOST=localhost
    PG_PORT=5442
    REDIS_URL=redis://localhost:16379
    REDIS_DATABASE=0
    JWT_ACCESS_SECRET=your_access_secret
    JWT_REFRESH_SECRET=your_refresh_secret
    JWT_ACCESS_EXPIRE=12h
    JWT_REFRESH_EXPIRE=28h
    JWT_REFRESH_EXPIRE_IN_MILLISECONDS=100800000
        ```
5.  **Start Docker containers (optional):**
    ```bash
    docker-compose up
    ```
6.  **Migrate the database:**
    ```bash
    npm run up
    # or to rollback migrations
    npm run down
    ```
7.  **Start the development server:**
    ```bash
    npm run start:dev
    ```
### Dependencies
-   **"@redis/client": "^1.1.1"**: Redis client for Node.js.
-   **"bcrypt": "^5.0.1"**: For password hashing.
-   **"cors": "^2.8.5"**:  Middleware to enable CORS.
-   **"db-migrate": "^0.11.13"**: Database migration tool.
-   **"db-migrate-pg": "^1.2.2"**: PostgreSQL driver for db-migrate.
-   **"dotenv": "^16.0.0"**:  Loads environment variables from a `.env` file.
-   **"express": "^4.17.3"**:  Web application framework.
-   **"express-joi-validation": "^5.0.1"**:  Middleware for request validation using Joi.
-   **"http-status-codes": "^2.2.0"**:  Provides HTTP status code constants.
-   **"joi": "^17.6.0"**:  Schema description language and validator.
-   **"jsonwebtoken": "^8.5.1"**:  JSON Web Token implementation.
-   **"morgan": "^1.10.0"**:  HTTP request logger middleware.
-   **"pg": "^8.7.3"**:  PostgreSQL client for Node.js.
-   **"redis": "^3.0.0"**:  Redis client (older version, consider upgrading to `@redis/client`).
-   **"socket.io": "^4.4.1"**:  Real-time, bidirectional communication.
-   **"swagger-ui-express": "^4.5.0"**:  Serves Swagger UI for API documentation.
-   **"ts-node": "^10.5.0"**:  TypeScript execution and REPL for Node.js.
-   **"typescript": "^4.5.5"**:  TypeScript compiler.
-   **"uuid": "^8.3.2"**:  For generating unique IDs.
## Code Structure
chat_app/
├── migrations/                  # Database migration files
├── src/                       # Source code
│   ├── controllers/           # Request handlers
│   │   ├── socket/            # Socket.IO controllers
│   │   ├── auth.controller.ts # Authentication logic
│   │   ├── chat.controller.ts # Chat logic
│   ├── database/              # Database connection setup
│   │   ├── queryPool.ts       # PostgreSQL connection pool
│   │   ├── redis.ts           # Redis client setup
│   ├── middleware/            # Middleware functions
│   │   ├── checkToken.ts      # Authentication middleware
│   │   ├── errorHandler.ts    # Error handling middleware
│   ├── models/                # Data models and interfaces
│   │   ├── enums/             # Enumerations
│   │   ├── interfaces/        # TypeScript interfaces
│   │   ├── requestModels/     # Request models for validation
│   ├── repository/            # Data access layer
│   │   ├── socket/            # Socket.IO repositories
│   │   ├── auth.repository.ts # Authentication queries
│   │   ├── chat.repository.ts # Chat queries
│   ├── routes/                # API route definitions
│   │   ├── auth.routes.ts     # Authentication routes
│   │   ├── chat.routes.ts     # Chat routes
│   │   ├── index.ts           # Main route index
│   ├── services/              # Business logic services
│   │   ├── error.service.ts   # Error handling service
│   │   ├── token.service.ts   # JWT token service
│   ├── socket/                # Socket.IO setup
│   │   ├── index.socket.ts    # WebSocket server setup
│   ├── validation/            # Request validation schemas
│   │   ├── socket/            # Socket.IO validation
│   │   ├── auth.validate.ts   # Authentication validation
│   │   ├── chat.validate.ts   # Chat validation
│   ├── app.ts                   # Main application entry point
├── swagger/                   # Swagger API documentation
│   ├── api/                   # API endpoint definitions
│   ├── models/                # Swagger model definitions
│   ├── components.js          # Swagger components
│   ├── path.js                # Swagger paths
├── tests/                     # API tests
│   ├── apiTesting/            # API test files
├── uploads/                   # Uploaded files directory
├── .env.sample                # Example environment variables
├── .gitignore                 # Git ignore file
├── database.json              # Database configuration
├── docker-compose.yml         # Docker Compose configuration
├── jest.config.js             # Jest configuration
├── nodemon.json               # Nodemon configuration
├── package.json               # Project dependencies and scripts
├── README.md                  # Project documentation
├── swagger.js                 # Swagger configuration
├── tsconfig.json              # TypeScript configuration
├── tslint.json                # TSLint configuration
## API Documentation
The API documentation is available through Swagger UI at `http://host:port/swagger`.
### Authentication Endpoints
#### 1. Signup (`POST /api/auth/signup`)
-   **Summary:** Registers a new user.
-   **Request Body:**
    ```json
    {
        "firstName": "John",
        "lastName": "Doe",
        "email": "john_1990@gmail.com",
        "password": "qwerty$john",
        "confirmPassword": "qwerty$john"
    }
    ```
-   **Response (200 OK):**
    ```json
    {
        "data": {
            "id": 10,
            "firstName": "John",
            "lastName": "Doe",
            "email": "john_doe@gmail.com",
            "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyMzc1OTI0LCJleHAiOjE2NjI0NjIzMjR9.Gl8H793f5Bzwur6c4XUxNLBhp0f1tlgG9Yh4A31km_k",
            "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyMzc1OTI0LCJleHAiOjE2NjI1NDg3MjR9.6bx-5LMBZFoyaIIUHvTwy5PNMj4u-lRm13npwxbRsxQ"
        },
        "success": true
    }
    ```
#### 2. Login (`POST /api/auth/login`)
-   **Summary:** Logs in an existing user.
-   **Request Body:**
    ```json
    {
        "email": "j_doe@gmail.com",
        "password": "John$Doe"
    }
    ```
-   **Response (200 OK):**
    ```json
    {
        "data": {
            "id": 10,
            "firstName": "John",
            "lastName": "Doe",
            "email": "john_doe@gmail.com",
            "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyMzc1OTI0LCJleHAiOjE2NjI0NjIzMjR9.Gl8H793f5Bzwur6c4XUxNLBhp0f1tlgG9Yh4A31km_k",
            "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyMzc1OTI0LCJleHAiOjE2NjI1NDg3MjR9.6bx-5LMBZFoyaIIUHvTwy5PNMj4u-lRm13npwxbRsxQ",
            "chats": [
                {
                    "interlocutor": {
                        "id": 52,
                        "firstName": "Mark",
                        "lastName": "Zuckerberg",
                        "email": "mark_meta@gmail.com"
                    },
                    "room": "2d52e031-7039-48bb-8052-056649670bd1"
                }
            ]
        },
        "success": true
    }
    ```
#### 3. Refresh Token (`POST /api/auth/refresh-token`)
-   **Summary:** Refreshes the access token using a refresh token.
-   **Request Body:**
    ```json
    {
        "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyNTQ4MDg5LCJleHAiOjE2NjI2NDg4ODl9.dtZ7bXFq3e2yY_5NO-NOBdrZKJ0WSGB_pVK3oCUco8A"
    }
    ```
-   **Response (200 OK):**
    ```json
    {
        "data": {
            "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyNTQ4MDg5LCJleHAiOjE2NjI1OTEyODl9.vZknab_yZLy-t_llXELvZUEsCvUAgcoXgsFY7Ava5y8",
            "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyNTQ4MDg5LCJleHAiOjE2NjI2NDg4ODl9.dtZ7bXFq3e2yY_5NO-NOBdrZKJ0WSGB_pVK3oCUco8A"
        },
        "success": true
    }
    ```
#### 4. Logout (`POST /api/auth/logout`)
-   **Summary:** Logs out a user by invalidating the refresh token.
-   **Request Body:**
    ```json
    {
        "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjYyNTQ4MDg5LCJleHAiOjE2NjI2NDg4ODl9.dtZ7bXFq3e2yY_5NO-NOBdrZKJ0WSGB_pVK3oCUco8A"
    }
    ```
-   **Headers:**
    -   `Authorization`: Bearer <access_token>
-   **Response (200 OK):**
    ```json
    {
        "data": {},
        "success": true
    }
    ```
### Chat Endpoints
#### 1. Create Chat (`POST /api/chat`)
-   **Summary:** Creates a new chat with a specified user.
-   **Request Body:**
    ```json
    {
        "interlocutorEmail": "mark_meta@gmail.com"
    }
    ```
-   **Headers:**
    -   `Authorization`: Bearer <access_token>
-   **Response (200 OK):**
    ```json
    {
        "data": {
            "interlocutor": {
                "id": 52,
                "firstName": "Mark",
                "lastName": "Zuckerberg",
                "email": "mark_meta@gmail.com"
            },
            "room": "2d52e031-7039-48bb-8052-056649670bd1"
        },
        "success": true
    }
    ```
#### 2. Get Chat List (`GET /api/chat`)
-   **Summary:** Retrieves a list of chats for the authenticated user.
-   **Headers:**
    -   `Authorization`: Bearer <access_token>
-   **Response (200 OK):**
    ```json
    {
        "data": [
            {
                "interlocutor": {
                    "id": 52,
                    "firstName": "Mark",
                    "lastName": "Zuckerberg",
                    "email": "mark_meta@gmail.com"
                },
                "room": "2d52e031-7039-48bb-8052-056649670bd1"
            }
        ],
        "success": true
    }
    ```
#### 3. Get Chat Messages (`GET /api/chat/{room}`)
-   **Summary:** Retrieves all messages for a specific chat room.
-   **Parameters:**
    -   `room` (path parameter): The chat room UUID.
-   **Headers:**
    -   `Authorization`: Bearer <access_token>
-   **Response (200 OK):**
    ```json
    {
        "data": [
            {
                "id": 52,
                "room": "2d52e031-7039-48bb-8052-056649670bd1",
                "date": "2022-07-25T15:26:00.781Z",
                "message": "Hi I'm John Doe",
                "filePath": "http://localhost:8050/files/2d52e031-7039-48bb-8052-056649670bd1.txt",
                "msgFromId": 10,
                "msgToId": 36
            }
        ],
        "success": true
    }
    ```
### Socket.IO API
The Socket.IO API is used for real-time communication.
-   **Endpoint:** `ws://host:port/chat`
-   **Headers:**
    -   `authorization`:  The user's access token.
#### Events
-   **`connect`**:  Fired when a client connects to the WebSocket server.
-   **`disconnect`**:  Fired when a client disconnects from the WebSocket server.
-   **`msg_receive`**:  Fired when a message is received.
-   **`error`**:  Fired when an error occurs.

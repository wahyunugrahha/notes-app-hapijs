# Notes App Backend (Hapi.js)

This repository hosts the backend for a Notes application, built using Hapi.js and PostgreSQL. It provides a RESTful API for managing user accounts, authenticating users, and handling personal notes with proper authorization.

## Table of Contents

- [Features](#features)
- [Technologies Used](#technologies-used)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Database Setup](#database-setup)
  - [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
  - [Users API](#users-api)
  - [Authentications API](#authentications-api)
  - [Notes API](#notes-api)
- [API Testing](#api-testing)

## Features

* **Users Management**:
  * Register new users with `username`, `password`, and `fullname`.
  * Retrieve user details by ID.
  * Ensures unique usernames.
* **Authentication & Authorization**:
  * User login and generation of Access and Refresh Tokens.
  * Token refresh mechanism using a valid Refresh Token.
  * Token invalidation (logout) by deleting the Refresh Token.
  * JWT-based authentication strategy for protected routes.
  * Authorization checks to ensure users can only access/modify their own notes.
* **Notes Management**:
  * Add new notes with `title`, `body`, and `tags`, associated with the authenticated user.
  * Retrieve all notes owned by the authenticated user.
  * Get a specific note by ID, with owner verification.
  * Edit note details by ID, with owner verification.
  * Delete notes by ID, with owner verification.
* **Robust Error Handling**: Utilizes custom error classes (`ClientError`, `InvariantError`, `NotFoundError`, `AuthenticationError`, `AuthorizationError`) for precise error feedback.
* **Data Validation**: Implements Joi for request payload validation across all endpoints.
* **PostgreSQL Database**: Uses PostgreSQL for data persistence, managed with `node-pg-migrate` for schema evolution.
* **Code Quality**: Enforced with ESLint (using `airbnb-base` configuration) and Prettier for consistent coding standards and automatic formatting.

## Technologies Used

* **Node.js**: JavaScript runtime environment.
* **Hapi.js**: A rich framework for building applications and services.
* **PostgreSQL**: Robust open-source relational database.
* **Joi**: Schema description language and validator.
* **nanoid**: Secure, URL-friendly unique string ID generator.
* **node-pg-migrate**: Database migration tool for PostgreSQL.
* **bcrypt**: Password hashing library.
* **@hapi/jwt**: JWT authentication plugin for Hapi.
* **dotenv**: Loads environment variables from `.env` file.
* **nodemon**: Development tool for automatic server restarts.
* **ESLint**: JavaScript linter for code quality.

## Getting Started

Follow these instructions to set up and run the project locally.

### Prerequisites

* Node.js (>= 10)
* PostgreSQL

### Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/wahyunugrahha/notes-app-hapijs.git
    cd notes-app-hapijs
    ```

2. **Install dependencies**:
    ```bash
    npm install
    ```

### Database Setup

1. **Create a PostgreSQL database**:
    ```sql
    CREATE DATABASE notesapp;
    ```

2. **Run migrations**:
    ```bash
    npm run migrate up
    ```

    This will create the `notes`, `users`, and `authentications` tables, and add the `owner` column to the `notes` table.

### Environment Variables

Create a `.env` file in the root directory of the project with the following content:

```dotenv
PORT=5000
HOST=localhost
PGUSER=your_pg_username
PGHOST=your_pg_host
PGDATABASE=notesapp
PGPASSWORD=your_pg_password
PGPORT=5432
ACCESS_TOKEN_KEY=your_access_token_secret
REFRESH_TOKEN_KEY=your_refresh_token_secret
ACCESS_TOKEN_AGE=1800
```

Replace `your_pg_username`, `your_pg_host`, `your_pg_password`, `your_access_token_secret`, and `your_refresh_token_secret` with your credentials and keys.

## Running the Application

To start the API server in development mode:

```bash
npm run start:dev
```

For production mode:

```bash
npm run start:prod
```

The server will run on the specified HOST and PORT (e.g., `http://localhost:5000`).

## API Endpoints

### Users API

| Method | Path         | Description              | Authentication Required |
|--------|--------------|--------------------------|--------------------------|
| POST   | /users       | Register a new user      | No                       |
| GET    | /users/{id}  | Get user details by ID   | No                       |

### Authentications API

| Method | Path               | Description                                    | Authentication Required |
|--------|--------------------|------------------------------------------------|--------------------------|
| POST   | /authentications   | Login and obtain Access/Refresh Tokens         | No                       |
| PUT    | /authentications   | Refresh Access Token using Refresh Token       | No                       |
| DELETE | /authentications   | Logout (invalidate Refresh Token)              | No                       |

### Notes API

| Method | Path         | Description                                    | Authentication Required |
|--------|--------------|------------------------------------------------|--------------------------|
| POST   | /notes       | Create a new note                              | Yes                      |
| GET    | /notes       | Get all notes owned by the authenticated user  | Yes                      |
| GET    | /notes/{id}  | Get a specific note by ID (owner only)         | Yes                      |
| PUT    | /notes/{id}  | Update a note by ID (owner only)               | Yes                      |
| DELETE | /notes/{id}  | Delete a note by ID (owner only)               | Yes                      |

## API Testing

This repository includes a Postman collection and environment for comprehensive API testing:

- `Notes API Test.postman_collection.json`: Contains all API requests and tests.
- `Notes API Test.postman_environment.json`: Contains environment variables used in testing.

You can import these files into Postman. Ensure the server is running and environment variables are properly configured before testing.
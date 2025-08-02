# OpenMusic API V1

This repository contains the backend for **OpenMusic API**, a robust and scalable music platform built with Node.js and PostgreSQL. It provides RESTful endpoints for managing albums and songs, including features for adding, retrieving, updating, and deleting musical content. The API also includes comprehensive validation and error handling.

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
  - [Albums API](#albums-api)
  - [Songs API](#songs-api)
- [API Testing](#api-testing)

## Features

* **Albums Management**:
  * Add new albums with `name` and `year`.
  * Retrieve album details, including associated songs.
  * Update existing album information.
  * Delete albums by ID.
* **Songs Management**:
  * Add new songs with `title`, `year`, `genre`, `performer`, `duration` (optional), and `albumId` (optional).
  * Retrieve all songs, with optional filtering by `title` and `performer`.
  * Get a specific song by ID.
  * Edit song details by ID.
  * Delete songs by ID.
* **Robust Error Handling**: Utilizes custom error classes (`ClientError`, `InvariantError`, `NotFoundError`) for better error management and user feedback.
* **Data Validation**: Implements Joi for request payload validation to ensure data integrity.
* **PostgreSQL Database**: Uses PostgreSQL for data persistence.
* **Code Quality**: Configured with ESLint and Prettier for consistent code style and error detection.

## Technologies Used

* **Node.js**: JavaScript runtime environment.
* **Hapi.js**: A rich framework for building applications and services.
* **PostgreSQL**: Relational database system.
* **Joi**: Powerful schema description language and data validator.
* **nanoid**: Tiny, secure, URL-friendly, unique string ID generator.
* **node-pg-migrate**: Database migration tool for PostgreSQL.
* **nodemon**: Utility that monitors for any changes in your source and automatically restarts your server.
* **dotenv**: Loads environment variables from a `.env` file.
* **ESLint**: Pluggable JavaScript linter.
* **globals**: Global variables from different JavaScript environments.

## Getting Started

Follow these instructions to set up and run the project locally.

### Prerequisites

* Node.js (>= 10)
* PostgreSQL

### Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/wahyunugrahha/openmusic-api.git
    cd openmusic-api
    ```

2. **Install dependencies**:
    ```bash
    npm install
    ```

### Database Setup

1. **Create a PostgreSQL database**:
    ```sql
    CREATE DATABASE openmusicapi;
    ```

2. **Run migrations**:
    The project uses `node-pg-migrate` for database migrations.
    ```bash
    npm run migrate up
    ```

    This will create the `albums` and `songs` tables.

### Environment Variables

Create a `.env` file in the root directory of the project with the following content:

```dotenv
PORT=5000
HOST=localhost
PGUSER=your_pg_username
PGHOST=your_pg_host
PGDATABASE=openmusicapi
PGPASSWORD=your_pg_password
PGPORT=5432
```

Replace `your_pg_username`, `your_pg_host`, and `your_pg_password` with your PostgreSQL credentials.

## Running the Application

To start the API server in development mode (with nodemon for automatic restarts):

```bash
npm start
```

The server will run on the specified HOST and PORT (e.g., `http://localhost:5000`).

## API Endpoints

### Albums API

| Method | Path          | Description             |
|--------|---------------|-------------------------|
| POST   | /albums       | Add a new album         |
| GET    | /albums/{id}  | Get album details by ID |
| PUT    | /albums/{id}  | Update an album by ID   |
| DELETE | /albums/{id}  | Delete an album by ID   |

### Songs API

| Method | Path          | Description                                        |
|--------|---------------|----------------------------------------------------|
| POST   | /songs        | Add a new song                                     |
| GET    | /songs        | Get all songs (with optional filters)              |
| GET    | /songs/{id}   | Get song details by ID                             |
| PUT    | /songs/{id}   | Update a song by ID                                |
| DELETE | /songs/{id}   | Delete a song by ID                                |

## API Testing

This repository includes a Postman collection and environment for testing the API endpoints.

* `Open Music API V1 Test.postman_collection.json`: Contains all the API requests and tests.
* `OpenMusic API Test.postman_environment.json`: Contains environment variables used in the Postman collection.

You can import these files into Postman to easily test the API functionality. Make sure the API server is running before executing the Postman tests.
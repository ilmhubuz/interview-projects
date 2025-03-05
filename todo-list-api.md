# Todo List API

## Project Description
Develop a simple RESTful API to manage a user's todo list. Users should be able to register, authenticate, and manage their tasks (CRUD operations). The API should enforce authentication for task management.

---

## Technologies Used (**Weight: 10%**)
- .NET 8
- ASP.NET Core Web API
- Entity Framework Core
- PostgreSQL
- JWT Authentication
- FluentValidation
- Swagger for API Documentation

---

## Prerequisites (**Weight: 5%**)
- .NET 8 SDK installed
- PostgreSQL database
- An IDE (Visual Studio, VS Code, or JetBrains Rider)

---

## Database Schema (**Weight: 15%**)

### Users Table
| Column Name  | Type        | Constraints |
|--------------|------------|-------------|
| Id           | Guid       | Primary Key |
| Username     | String(50) | Required, Unique |
| PasswordHash | String(255)| Required |

### Todos Table
| Column Name | Type       | Constraints |
|-------------|-----------|-------------|
| Id          | Guid      | Primary Key |
| Title       | String(255) | Required |
| Description | String(1000) | Optional |
| IsCompleted | Boolean   | Default: false |
| UserId      | Guid      | Foreign Key (Users.Id) |

---

## API Endpoints (**Weight: 30%**)

### Authentication (**Weight: 10%**)
#### Register a new user
- **Endpoint:** `POST /api/auth/register`
- **Request Body:**
  ```json
  {
    "username": "testuser",
    "password": "Secure123!"
  }
  ```
- **Responses:**
  - `201 Created`
  - `400 Bad Request` (Validation errors)

#### Login and obtain JWT token
- **Endpoint:** `POST /api/auth/login`
- **Request Body:**
  ```json
  {
    "username": "testuser",
    "password": "Secure123!"
  }
  ```
- **Responses:**
  - `200 OK` (JWT Token)
  - `401 Unauthorized`

### Todo Management (Authenticated) (**Weight: 20%**)
#### Create a Todo
- **Endpoint:** `POST /api/todos`
- **Authorization:** Requires JWT Token
- **Request Body:**
  ```json
  {
    "title": "Buy groceries",
    "description": "Milk, eggs, and bread"
  }
  ```
- **Responses:**
  - `201 Created`
  - `400 Bad Request`

#### Get All Todos
- **Endpoint:** `GET /api/todos`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `200 OK` (List of todos)

#### Get a Single Todo
- **Endpoint:** `GET /api/todos/{id}`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `200 OK`
  - `404 Not Found`

#### Update a Todo
- **Endpoint:** `PUT /api/todos/{id}`
- **Authorization:** Requires JWT Token
- **Request Body:**
  ```json
  {
    "title": "Buy groceries",
    "description": "Updated list",
    "isCompleted": true
  }
  ```
- **Responses:**
  - `204 No Content`
  - `400 Bad Request`
  - `404 Not Found`

#### Delete a Todo
- **Endpoint:** `DELETE /api/todos/{id}`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `204 No Content`
  - `404 Not Found`

---

## Security (**Weight: 10%**)
- **Password Hashing**: Store passwords securely using hashing.
- **JWT Authentication**: Protect endpoints and ensure only authenticated users can manage todos.
- **User Isolation**: Users should only be able to access their own todos.

---

## Getting Started (**Weight: 5%**)
1. Clone the repository.
2. Configure PostgreSQL connection string in `appsettings.json`.
3. Apply migrations:
   ```bash
   dotnet ef database update
   ```
4. Run the application:
   ```bash
   dotnet run
   ```
5. Access Swagger UI at `/swagger/index.html` for API documentation.

---

## Total Estimated Time
**3-4 hours**

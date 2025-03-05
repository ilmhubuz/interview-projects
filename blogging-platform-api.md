# Blogging Platform API (Advanced)

## Project Description
Develop a robust blogging platform API where users can register, create posts, comment on posts, like posts, and more. The API supports advanced features including content categorization, tagging, media attachments, role-based access control, post scheduling (drafts and published posts), advanced search and filtering, and notifications for post activities. This API is secured using JWT authentication and enforces permissions based on user roles.

---

## Technologies Used (**Weight: 10%**)
- .NET 8
- ASP.NET Core Web API
- Entity Framework Core
- PostgreSQL
- JWT Authentication
- FluentValidation
- Swagger for API Documentation
- SignalR (for real-time notifications)

---

## Prerequisites (**Weight: 5%**)
- .NET 8 SDK installed
- PostgreSQL database
- An IDE (Visual Studio, VS Code, or JetBrains Rider)

---

## Database Schema (**Weight: 15%**)

### Users Table
| Column Name  | Type        | Constraints                       |
|--------------|-------------|-----------------------------------|
| Id           | Guid        | Primary Key                       |
| Username     | String(50)  | Required, Unique                  |
| PasswordHash | String(255) | Required                          |
| Role         | String(20)  | Required (e.g., Admin, Author, Moderator, Reader) |

### Posts Table
| Column Name   | Type         | Constraints                                      |
|---------------|--------------|--------------------------------------------------|
| Id            | Guid         | Primary Key                                      |
| Title         | String(255)  | Required                                         |
| Content       | Text         | Required                                         |
| Status        | String(20)   | Required (Draft, Published)                      |
| CreatedAt     | DateTime     | Default: Now                                     |
| PublishedAt   | DateTime     | Nullable (set when post is published)            |
| UserId        | Guid         | Foreign Key (Users.Id)                           |

### Categories Table
| Column Name | Type         | Constraints                |
|-------------|--------------|----------------------------|
| Id          | Guid         | Primary Key                |
| Name        | String(100)  | Required, Unique           |

### Tags Table
| Column Name | Type         | Constraints                |
|-------------|--------------|----------------------------|
| Id          | Guid         | Primary Key                |
| Name        | String(100)  | Required, Unique           |

### PostTags Table (Join Table for Posts and Tags)
| Column Name | Type  | Constraints                                   |
|-------------|-------|-----------------------------------------------|
| PostId      | Guid  | Foreign Key (Posts.Id)                        |
| TagId       | Guid  | Foreign Key (Tags.Id)                         |

### Comments Table
| Column Name  | Type        | Constraints                       |
|--------------|-------------|-----------------------------------|
| Id           | Guid        | Primary Key                       |
| Content      | Text        | Required                          |
| CreatedAt    | DateTime    | Default: Now                      |
| PostId       | Guid        | Foreign Key (Posts.Id)            |
| UserId       | Guid        | Foreign Key (Users.Id)            |

### Likes Table
| Column Name  | Type        | Constraints                       |
|--------------|-------------|-----------------------------------|
| Id           | Guid        | Primary Key                       |
| PostId       | Guid        | Foreign Key (Posts.Id)            |
| UserId       | Guid        | Foreign Key (Users.Id)            |

### Media Attachments Table
| Column Name  | Type         | Constraints                                      |
|--------------|--------------|--------------------------------------------------|
| Id           | Guid         | Primary Key                                      |
| FileUrl      | String(500)  | Required                                         |
| FileType     | String(50)   | Required (e.g., image, video)                    |
| PostId       | Guid         | Foreign Key (Posts.Id)                           |

### Notifications Table
| Column Name  | Type         | Constraints                                      |
|--------------|--------------|--------------------------------------------------|
| Id           | Guid         | Primary Key                                      |
| UserId       | Guid         | Foreign Key (Users.Id)                           |
| Message      | String(500)  | Required                                         |
| IsRead       | Boolean      | Default: false                                   |
| CreatedAt    | DateTime     | Default: Now                                     |

---

## API Endpoints (**Weight: 30%**)

### Authentication (**Weight: 10%**)
#### Register a new user
- **Endpoint:** `POST /api/auth/register`
- **Request Body:**
  ```json
  {
    "username": "testuser",
    "password": "Secure123!",
    "role": "Author"
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

### Post Management (**Weight: 10%**)
#### Create a Post
- **Endpoint:** `POST /api/posts`
- **Authorization:** Requires JWT Token (Role: Author or Admin)
- **Request Body:**
  ```json
  {
    "title": "My First Blog Post",
    "content": "This is the content of the blog post.",
    "status": "Draft",
    "categoryId": "GUID-of-category",
    "tagIds": ["GUID-of-tag1", "GUID-of-tag2"]
  }
  ```
- **Explanation:** Users can save posts as drafts and later publish them. Categories and tags help organize content.
- **Responses:**
  - `201 Created`
  - `400 Bad Request`

#### Update a Post (Only Owner or Admin)
- **Endpoint:** `PUT /api/posts/{id}`
- **Authorization:** Requires JWT Token
- **Request Body:**
  ```json
  {
    "title": "Updated Blog Title",
    "content": "Updated content.",
    "status": "Published",
    "publishedAt": "2025-03-06T12:00:00Z",
    "categoryId": "GUID-of-category",
    "tagIds": ["GUID-of-tag1", "GUID-of-tag2"]
  }
  ```
- **Responses:**
  - `204 No Content`
  - `403 Forbidden` (If not the owner or admin)
  - `404 Not Found`

#### Delete a Post (Only Owner or Admin)
- **Endpoint:** `DELETE /api/posts/{id}`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `204 No Content`
  - `403 Forbidden`
  - `404 Not Found`

#### Get All Posts with Filtering and Search
- **Endpoint:** `GET /api/posts`
- **Query Parameters:**
  - `status`: Filter by post status (Draft, Published)
  - `categoryId`: Filter by category
  - `tagId`: Filter by a specific tag
  - `search`: Search keyword in title or content
- **Responses:**
  - `200 OK` (List of posts matching the criteria)

#### Get a Single Post
- **Endpoint:** `GET /api/posts/{id}`
- **Responses:**
  - `200 OK`
  - `404 Not Found`

### Comments (**Weight: 5%**)
#### Add a Comment
- **Endpoint:** `POST /api/posts/{postId}/comments`
- **Authorization:** Requires JWT Token
- **Request Body:**
  ```json
  {
    "content": "Great post!"
  }
  ```
- **Responses:**
  - `201 Created`
  - `400 Bad Request`

### Likes (**Weight: 5%**)
#### Like a Post
- **Endpoint:** `POST /api/posts/{postId}/like`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `200 OK`
  - `400 Bad Request` (If already liked)

#### Unlike a Post
- **Endpoint:** `DELETE /api/posts/{postId}/like`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `204 No Content`
  - `400 Bad Request` (If not liked before)

### Media Attachments (**Weight: 5%**)
#### Upload Media for a Post
- **Endpoint:** `POST /api/posts/{postId}/media`
- **Authorization:** Requires JWT Token (Owner or Admin)
- **Request Body:** (multipart/form-data)
  - File upload field for media file
  - Additional field for `fileType` (e.g., image, video)
- **Responses:**
  - `201 Created`
  - `400 Bad Request`

### Notifications (**Weight: 5%**)
#### Get Notifications
- **Endpoint:** `GET /api/notifications`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `200 OK` (List of notifications for the user)

#### Mark Notification as Read
- **Endpoint:** `PUT /api/notifications/{id}/read`
- **Authorization:** Requires JWT Token
- **Responses:**
  - `204 No Content`
  - `404 Not Found`

---

## Security & Role-Based Access (**Weight: 10%**)
- **Password Hashing:** Secure password storage using hashing.
- **JWT Authentication:** Ensures endpoints are protected.
- **Role-Based Access:** 
  - **Admin:** Full access to all posts and user management.
  - **Author:** Can create, update, and delete their own posts.
  - **Moderator:** Can manage comments and moderate content.
- **User Isolation:** Users can only modify their own content unless elevated privileges apply.

---

## Additional Features Explained
- **Post Scheduling:**  
  Authors can save posts as drafts and later update the status to "Published". The `PublishedAt` field allows scheduling the publication.
- **Content Organization:**  
  Categories and Tags help classify posts. The many-to-many relationship between posts and tags enables flexible content grouping.
- **Media Attachments:**  
  Allows authors to attach images or videos to posts. The API handles file uploads and stores file URLs for retrieval.
- **Advanced Search & Filtering:**  
  Clients can filter posts by status, category, tag, or perform a keyword search over titles and content.
- **Real-Time Notifications:**  
  Using SignalR, the API can push notifications (such as new comments or likes) to users in real-time.
- **Role-Based Security:**  
  Different user roles control access to certain endpoints. Admins can manage all resources while authors are limited to their own posts, and moderators can handle comments.

---

## Getting Started (**Weight: 5%**)
1. Clone the repository.
2. Configure the PostgreSQL connection string in `appsettings.json`.
3. Apply migrations:
   ```bash
   dotnet ef database update
   ```
4. Run the application:
   ```bash
   dotnet run
   ```
5. Access Swagger UI at `/swagger/index.html` for API documentation.
6. For real-time notifications, connect via SignalR client as documented.

---

## Total Estimated Time
**5-6 hours**
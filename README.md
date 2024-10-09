# Library Management System Documentation

## Overview
The Library Management System is a web application designed to facilitate the management of books, users, and orders in a library environment. 
It supports various features to streamline library operations.

## Table of Contents
1. [Database Structure](#database-structure)
2. [API Documentation](#api-documentation)
3. [Frontend Flow](#frontend-flow)
4. [Hosting Instructions](#hosting-instructions)

---

## 1. Database Structure

### 1.1. Table Structures

#### 1.1.1. Users Table
| Column Name  | Data Type   | Constraints         | Description                  |
|--------------|-------------|---------------------|------------------------------|
| id           | INT         | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the user |
| firstName    | VARCHAR(50) | NOT NULL            | First name of the user      |
| lastName     | VARCHAR(50) | NOT NULL            | Last name of the user       |
| email        | VARCHAR(100)| UNIQUE, NOT NULL    | Email address of the user    |
| password     | VARCHAR(255)| NOT NULL            | Password for user authentication |
| role         | ENUM        | NOT NULL (e.g., 'user', 'librarian') | Role of the user in the system |
| createdAt    | DATETIME    | DEFAULT CURRENT_TIMESTAMP | Timestamp when user was created |
| updatedAt    | DATETIME    | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Timestamp when user details were last updated |

#### 1.1.2. Books Table
| Column Name      | Data Type   | Constraints         | Description                  |
|------------------|-------------|---------------------|------------------------------|
| id                | INT         | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the book |
| title             | VARCHAR(255)| NOT NULL            | Title of the book           |
| author            | VARCHAR(100)| NOT NULL            | Author of the book          |
| price             | DECIMAL(10,2)| NOT NULL           | Price of the book           |
| bookCategoryId    | INT         | FOREIGN KEY         | References the category ID of the book |
| ordered           | BOOLEAN     | DEFAULT FALSE       | Indicates if the book is ordered |
| createdAt         | DATETIME    | DEFAULT CURRENT_TIMESTAMP | Timestamp when the book was added |
| updatedAt         | DATETIME    | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Timestamp when book details were last updated |

#### 1.1.3. Categories Table
| Column Name  | Data Type   | Constraints         | Description                  |
|--------------|-------------|---------------------|------------------------------|
| id           | INT         | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the category |
| category     | VARCHAR(100)| NOT NULL            | Main category name           |
| subCategory  | VARCHAR(100)| NOT NULL            | Subcategory name             |
| createdAt    | DATETIME    | DEFAULT CURRENT_TIMESTAMP | Timestamp when category was created |
| updatedAt    | DATETIME    | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Timestamp when category details were last updated |

### 1.2. Database Relationships
- **Users** can borrow multiple **Books**.
- **Books** belong to a single **Category**.
- **Categories** can have multiple **Books**.

#### 1.3. Database Diagram
(Provide a visual representation of the database structure here)

```plaintext
+-----------------+
|     Users       |
|-----------------|
| id (PK)         |
| firstName       |
| lastName        |
| email           |
| password        |
| role            |
| createdAt       |
| updatedAt       |
+-----------------+
          |
          | 1:N
          |
+-----------------+
|     Books       |
|-----------------|
| id (PK)         |
| title           |
| author          |
| price           |
| bookCategoryId  |
| ordered         |
| createdAt       |
| updatedAt       |
+-----------------+
          |
          | N:1
          |
+-----------------+
|    Categories    |
|-----------------|
| id (PK)         |
| category        |
| subCategory     |
| createdAt       |
| updatedAt       |
+-----------------+
```

---

## 2. API Documentation

### 2.1. Authentication API

- **POST /api/Library/Login**
  - **Requirements**: 
    - Request Body: 
      ```json
      {
          "email": "user@example.com",
          "password": "yourpassword"
      }
      ```
  - **Output**: 
    - Success: 
      ```json
      {
          "token": "jwt_token"
      }
      ```
    - Errors: 
      - 401 Unauthorized: Invalid credentials.
      - 400 Bad Request: Missing required fields.

### 2.2. Users API

- **GET /api/Library/GetUsers**
  - **Output**:
    - Success: 
      ```json
      [
          {
              "id": 1,
              "firstName": "John",
              "lastName": "Doe",
              "email": "john.doe@example.com",
              "role": "user",
              ...
          }
      ]
      ```
    - Errors:
      - 500 Internal Server Error: Unable to retrieve users.

- **DELETE /api/Library/DeleteUsers**
  - **Requirements**: 
    - Query Parameter: `id`
  - **Output**: 
    - Success: 
      - 200 OK: "deleted"
    - Errors:
      - 404 Not Found: User does not exist.
      - 500 Internal Server Error: Unable to delete user.

### 2.3. Books API

- **GET /api/Library/GetBooks**
  - **Output**: 
    - Success: 
      ```json
      [
          {
              "id": 1,
              "title": "Book Title",
              "author": "Author Name",
              "price": 19.99,
              "bookCategoryId": 1,
              ...
          }
      ]
      ```
    - Errors:
      - 500 Internal Server Error: Unable to retrieve books.

- **POST /api/Library/AddBook**
  - **Requirements**: 
    - Request Body: 
      ```json
      {
          "title": "Book Title",
          "author": "Author Name",
          "price": 19.99,
          "bookCategoryId": 1
      }
      ```
  - **Output**: 
    - Success: 
      - 201 Created: "inserted"
    - Errors:
      - 400 Bad Request: Missing required fields.
      - 500 Internal Server Error: Unable to add book.

---
## Conclusion

This documentation provides an outro of the Library Management System project, covering all the aspects like the database structure, API endpoints, frontend user flow, and hosting instructions.

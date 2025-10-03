# IddirNet API Documentation

## Overview

This documentation covers the RESTful API for IddirNet, a platform for managing community associations (Iddirs). The API supports CRUD operations for key resources such as users, iddirs, members, and contributions.

- **Base URL**: `https://iddirnet-75e4b810c131.herokuapp.com/api/v1`
- **Authentication**: Bearer token in `Authorization` header (obtain via `/auth/login`).
- **Content-Type**: `application/json` for all requests and responses.
- **Error Handling**: Standard HTTP status codes; error responses include `{"error": "message", "details": {...}}`.

Use tools like Postman or curl for testing. Examples below use curl commands.

## Authentication

### Login (POST /auth/login)

Authenticate a user and receive a JWT token.

**Description**: Logs in a user with email and password.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepassword"
  }'
```

**Response (200 OK)**:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

### Register (POST /auth/register)

Create a new user account.

**Description**: Registers a new user.

**Request Body**:
```json
{
  "email": "newuser@example.com",
  "password": "securepassword",
  "name": "Jane Doe",
  "phone": "+1234567890"
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "newuser@example.com",
    "password": "securepassword",
    "name": "Jane Doe",
    "phone": "+1234567890"
  }'
```

**Response (201 Created)**:
```json
{
  "id": 2,
  "email": "newuser@example.com",
  "name": "Jane Doe",
  "phone": "+1234567890"
}
```

## Users

CRUD operations for user management.

### List Users (GET /users)

Retrieve a list of all users.

**Description**: Fetches users with pagination support.

**Query Parameters**:
- `page` (int, optional): Page number (default: 1).
- `limit` (int, optional): Items per page (default: 10).

**cURL Example**:
```bash
curl -X GET "https://iddirnet-75e4b810c131.herokuapp.com/api/v1/users?page=1&limit=5" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "users": [
    {
      "id": 1,
      "email": "user@example.com",
      "name": "John Doe",
      "phone": "+1234567890"
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 5
}
```

### Get User (GET /users/{id})

Retrieve a specific user by ID.

**Description**: Fetches details of a single user.

**Path Parameters**:
- `id` (int, required): User ID.

**cURL Example**:
```bash
curl -X GET https://iddirnet-75e4b810c131.herokuapp.com/api/v1/users/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "phone": "+1234567890"
}
```

### Create User (POST /users)

Create a new user (admin only).

**Description**: Adds a new user to the system.

**Request Body**:
```json
{
  "email": "admin@example.com",
  "password": "securepassword",
  "name": "Admin User",
  "phone": "+0987654321",
  "role": "admin"
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/users \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "securepassword",
    "name": "Admin User",
    "phone": "+0987654321",
    "role": "admin"
  }'
```

**Response (201 Created)**:
```json
{
  "id": 3,
  "email": "admin@example.com",
  "name": "Admin User",
  "phone": "+0987654321",
  "role": "admin"
}
```

### Update User (PUT /users/{id})

Update an existing user.

**Description**: Updates user details.

**Path Parameters**:
- `id` (int, required): User ID.

**Request Body**:
```json
{
  "name": "Updated John Doe",
  "phone": "+1111111111"
}
```

**cURL Example**:
```bash
curl -X PUT https://iddirnet-75e4b810c131.herokuapp.com/api/v1/users/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated John Doe",
    "phone": "+1111111111"
  }'
```

**Response (200 OK)**:
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "Updated John Doe",
  "phone": "+1111111111"
}
```

### Delete User (DELETE /users/{id})

Delete a user.

**Description**: Removes a user from the system.

**Path Parameters**:
- `id` (int, required): User ID.

**cURL Example**:
```bash
curl -X DELETE https://iddirnet-75e4b810c131.herokuapp.com/api/v1/users/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (204 No Content)**: Empty body.

## Iddirs

CRUD for Iddir groups (community associations).

### List Iddirs (GET /iddirs)

Retrieve all Iddirs.

**Query Parameters**:
- `page` (int, optional): Page number.
- `limit` (int, optional): Items per page.

**cURL Example**:
```bash
curl -X GET "https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs?page=1&limit=10" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "iddirs": [
    {
      "id": 1,
      "name": "Local Iddir Group",
      "description": "Community savings group",
      "location": "Addis Ababa"
    }
  ],
  "total": 1
}
```

### Get Iddir (GET /iddirs/{id})

Get specific Iddir details.

**Path Parameters**:
- `id` (int, required): Iddir ID.

**cURL Example**:
```bash
curl -X GET https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "id": 1,
  "name": "Local Iddir Group",
  "description": "Community savings group",
  "location": "Addis Ababa"
}
```

### Create Iddir (POST /iddirs)

Create a new Iddir.

**Request Body**:
```json
{
  "name": "New Iddir",
  "description": "New community group",
  "location": "Bahir Dar",
  "adminUserId": 1
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "New Iddir",
    "description": "New community group",
    "location": "Bahir Dar",
    "adminUserId": 1
  }'
```

**Response (201 Created)**:
```json
{
  "id": 2,
  "name": "New Iddir",
  "description": "New community group",
  "location": "Bahir Dar"
}
```

### Update Iddir (PUT /iddirs/{id})

Update Iddir details.

**Path Parameters**:
- `id` (int, required): Iddir ID.

**Request Body**:
```json
{
  "name": "Updated Iddir",
  "description": "Updated description"
}
```

**cURL Example**:
```bash
curl -X PUT https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Iddir",
    "description": "Updated description"
  }'
```

**Response (200 OK)**:
```json
{
  "id": 1,
  "name": "Updated Iddir",
  "description": "Updated description",
  "location": "Addis Ababa"
}
```

### Delete Iddir (DELETE /iddirs/{id})

Delete an Iddir.

**Path Parameters**:
- `id` (int, required): Iddir ID.

**cURL Example**:
```bash
curl -X DELETE https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (204 No Content)**: Empty body.

## Members

CRUD for managing members in Iddirs.

### List Members (GET /iddirs/{iddirId}/members)

List members of an Iddir.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.

**cURL Example**:
```bash
curl -X GET https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/members \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "members": [
    {
      "id": 1,
      "userId": 1,
      "iddirId": 1,
      "joinDate": "2025-01-01T00:00:00Z",
      "status": "active"
    }
  ]
}
```

### Add Member (POST /iddirs/{iddirId}/members)

Add a member to an Iddir.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.

**Request Body**:
```json
{
  "userId": 2,
  "joinDate": "2025-01-01T00:00:00Z"
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/members \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "userId": 2,
    "joinDate": "2025-01-01T00:00:00Z"
  }'
```

**Response (201 Created)**:
```json
{
  "id": 2,
  "userId": 2,
  "iddirId": 1,
  "joinDate": "2025-01-01T00:00:00Z",
  "status": "active"
}
```

### Remove Member (DELETE /iddirs/{iddirId}/members/{memberId})

Remove a member from an Iddir.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.
- `memberId` (int, required): Member ID.

**cURL Example**:
```bash
curl -X DELETE https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/members/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (204 No Content)**: Empty body.

## Contributions

CRUD for tracking contributions/payments to Iddirs.

### List Contributions (GET /iddirs/{iddirId}/contributions)

List contributions for an Iddir.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.

**Query Parameters**:
- `memberId` (int, optional): Filter by member.

**cURL Example**:
```bash
curl -X GET "https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/contributions?memberId=1" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (200 OK)**:
```json
{
  "contributions": [
    {
      "id": 1,
      "memberId": 1,
      "iddirId": 1,
      "amount": 100.00,
      "date": "2025-02-01T00:00:00Z",
      "description": "Monthly contribution"
    }
  ]
}
```

### Create Contribution (POST /iddirs/{iddirId}/contributions)

Record a new contribution.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.

**Request Body**:
```json
{
  "memberId": 1,
  "amount": 100.00,
  "date": "2025-02-01T00:00:00Z",
  "description": "Monthly contribution"
}
```

**cURL Example**:
```bash
curl -X POST https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/contributions \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "memberId": 1,
    "amount": 100.00,
    "date": "2025-02-01T00:00:00Z",
    "description": "Monthly contribution"
  }'
```

**Response (201 Created)**:
```json
{
  "id": 1,
  "memberId": 1,
  "iddirId": 1,
  "amount": 100.00,
  "date": "2025-02-01T00:00:00Z",
  "description": "Monthly contribution"
}
```

### Update Contribution (PUT /iddirs/{iddirId}/contributions/{id})

Update a contribution.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.
- `id` (int, required): Contribution ID.

**Request Body**:
```json
{
  "amount": 150.00,
  "description": "Updated contribution"
}
```

**cURL Example**:
```bash
curl -X PUT https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/contributions/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 150.00,
    "description": "Updated contribution"
  }'
```

**Response (200 OK)**:
```json
{
  "id": 1,
  "memberId": 1,
  "iddirId": 1,
  "amount": 150.00,
  "date": "2025-02-01T00:00:00Z",
  "description": "Updated contribution"
}
```

### Delete Contribution (DELETE /iddirs/{iddirId}/contributions/{id})

Delete a contribution.

**Path Parameters**:
- `iddirId` (int, required): Iddir ID.
- `id` (int, required): Contribution ID.

**cURL Example**:
```bash
curl -X DELETE https://iddirnet-75e4b810c131.herokuapp.com/api/v1/iddirs/1/contributions/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Response (204 No Content)**: Empty body.

## Error Responses

Common error examples:

- **401 Unauthorized**:
  ```json:disable-run
  {
    "error": "Unauthorized",
    "details": "Invalid token"
  }
  ```

-- **401 Unauthorized**:
  ```json
  {
    "error": "Unauthorized",
    "details": "Invalid token"
  }

- **500 Internal Server Error**:
  ```json
  {
    "error": "Internal Server Error",
    "details": "Unexpected error occurred"
  }
  ```

For more details, refer to the [Swagger UI](https://iddirnet-75e4b810c131.herokuapp.com/swagger/) or [Postman Collection](https://documenter.getpostman.com/view/45526338/2sB3HqHdvd).
```
# User Registration Endpoint

## Endpoint

`POST /user/register`

## Description

This endpoint allows a new user to register by providing their **full name, email, and password**.
It validates the input, hashes the password for security, saves the user into the database, and returns a **JWT authentication token** along with the created user details.

---

## Request Body

The request body must be in **JSON format** with the following structure:

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "johndoe@example.com",
  "password": "securePassword123"
}
```

### Field Requirements

- **fullname.firstname**: (String) Required, minimum 3 characters.
- **fullname.lastname**: (String) Optional.
- **email**: (String) Required, must be a valid email address.
- **password**: (String) Required, minimum 6 characters.

---

## Response

### Success Response (200 OK)

If the registration is successful, the server responds with:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "user": {
    "_id": "650f15be1c23c9a124c93e7d",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "johndoe@example.com",
    "socketId": null
  }
}
```

- **token**: A JWT token that can be used for authentication in subsequent requests.
- **user**: The newly created user object (password is excluded for security).

---

## Error Responses

### 400 Bad Request

Returned when validation fails (e.g., missing fields, invalid email, short password, etc.).

```json
{
  "errors": [
    {
      "msg": "First name must be at least 3 characters long",
      "param": "fullname.firstname",
      "location": "body"
    }
  ]
}
```

### 500 Internal Server Error

Returned when something goes wrong on the server side.

```json
{
  "error": "All fields are required"
}
```

---

## Notes

- The password is automatically hashed before being stored in the database.
- The returned JWT token is signed using the server’s secret key (`process.env.JWT_SECRET`).
- The password field will **never be returned** in any response for security reasons.

# User Login Endpoint

## Endpoint

`POST /user/login`

## Description

This endpoint allows an existing user to **log in** by providing their email and password.
On successful authentication, it returns a **JWT authentication token** along with the user details.

---

## Request Body

The request body must be in **JSON format**:

```json
{
  "email": "johndoe@example.com",
  "password": "securePassword123"
}
```

### Field Requirements

- **email**: (String) Required, must be a valid email address.
- **password**: (String) Required, minimum 6 characters.

---

## Response

### Success Response (200 OK)

If the login is successful, the server responds with:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "user": {
    "_id": "650f15be1c23c9a124c93e7d",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "johndoe@example.com",
    "socketId": null
  }
}
```

- **token**: A JWT token to be used for authenticated requests.
- **user**: The user object without the password field.

---

## Error Responses

### 400 Bad Request

Returned when validation fails (e.g., missing fields, invalid email format, or short password).

```json
{
  "errors": [
    {
      "msg": "Password must be atleast 6 characters long",
      "param": "password",
      "location": "body"
    }
  ]
}
```

### 401 Unauthorized

Returned when email or password is incorrect.

```json
{
  "message": "Invalid email or password"
}
```

---

## Notes

- The password provided in the request is compared with the **hashed password** stored in the database.
- If authentication succeeds, a signed **JWT token** is generated using `process.env.JWT_SECRET`.
- The `password` field is always excluded from the returned user object for security.

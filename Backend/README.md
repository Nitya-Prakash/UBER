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

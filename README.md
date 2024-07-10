# Go Gin JWT Authentication Project

This project demonstrates a simple API built using the Go programming language and the Gin web framework. The API includes user authentication via JWT (JSON Web Token).

## Features

- User login with Basic Auth
- JWT token generation
- Secure resource access using JWT

## Prerequisites

- Go 1.16 or higher
- Gin Web Framework
- Golang JWT package

## Installation

1. Clone the repository:

```bash
git clone https://github.com/Jacob-Krueckl/authplayground.git
cd authplayground
```

2. Install dependencies:

```bash
go mod tidy
```

## Running the Application

To start the application, run:

```bash
go run main.go
```

The server will start on localhost:8080.

## Endpoints

### Login

- URL: /login
- Method: POST
- Auth: Basic Auth
    - `Username`: admin
    - `Password`: secret
- Response:
    - 200 OK on success, returns a JWT token in the response body.

```json
{
    "token": "your_jwt_token_here"
}
```

### Access Resource

- URL: /resource
- Method: GET
- Auth: Bearer Token
    - Include the JWT token in the Authorization header as Bearer <your_jwt_token>.
- Response:
    - 200 OK on success, returns the secured resource data.
    - 401 Unauthorized if the token is invalid or expired.
    - 400 Bad Request if the token is not provided or malformed.
```json
{
    "data": "resource"
}
```

## Code Overview

### Main Function

The main function initializes the Gin router and defines the routes for login and accessing the secured resource.
Login Route

The `/login` route is protected with Basic Auth. On successful authentication, it generates a JWT token using the generateJWT function and returns it in the response.
Resource Route

The `/resource` route checks for a valid JWT token in the Authorization header. If the token is valid, it allows access to the secured resource.
JWT Token Generation

The `generateJWT` function creates a new JWT token with an expiration time of 5 minutes.

```go
func generateJWT() (string, error) {
    expirationTime := time.Now().Add(5 * time.Minute)
    claims := &Claims{
        Username: "username",
        RegisteredClaims: jwt.RegisteredClaims{
            ExpiresAt: jwt.NewNumericDate(expirationTime),
        },
    }

    token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
    return token.SignedString(jwtKey)
}
```

Contact

For any issues or suggestions, please open an issue on the GitHub repository.

Acknowledgements

- [Gin Web Framework](https://github.com/gin-gonic/gin)
- [Golang JWT](https://github.com/golang-jwt/jwt)
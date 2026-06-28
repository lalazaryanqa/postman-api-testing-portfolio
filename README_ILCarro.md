# ILCarro API Postman Tests

This folder contains Postman API tests for the ILCarro backend application.

## Swagger

```text
https://ilcarro-backend.herokuapp.com/swagger-ui/index.html
```

## Base URL

```text
https://ilcarro-backend.herokuapp.com
```

## Tested Controllers

- Authentication Controller
- Car Controller

---

## Collection Variables

```text
BaseURL_ILCARRO
email
password
token
serialNumberAlreadyExist
serialNumber
```

## Authentication Flow

### Registration

Creates a new user.

```text
POST {{BaseURL_ILCARRO}}/v1/user/registration/usernamepassword
```

Example body:

```json
{
  "username": "{{$randomEmail}}",
  "password": "Password123!",
  "firstName": "{{$randomFirstName}}",
  "lastName": "{{$randomLastName}}"
}
```

Expected status:

```text
200 OK
```

The registration script saves:

```text
email
password
```

---

### Login

Logs in with the registered user and saves JWT token.

```text
POST {{BaseURL_ILCARRO}}/v1/user/login/usernamepassword
```

Example body:

```json
{
  "username": "{{email}}",
  "password": "{{password}}"
}
```

Expected status:

```text
200 OK
```

The login script saves:

```text
token
```

---

## Car Controller Tests

### Add New Car

Creates a new car.

```text
POST {{BaseURL_ILCARRO}}/v1/cars
```

Authorization:

```text
Bearer {{token}}
```

Expected status:

```text
200 OK
```

Expected response includes:

```text
Car added successfully
```

---

### Delete Car

Deletes a car by serial number.

```text
DELETE {{BaseURL_ILCARRO}}/v1/cars/{{serialNumber}}
```

Authorization:

```text
Bearer {{token}}
```

Expected status:

```text
200 OK
```

Expected response includes:

```text
Car deleted successfully
```

---

### Get All User Cars

Gets all cars created by the current user.

```text
GET {{BaseURL_ILCARRO}}/v1/cars/my
```

Authorization:

```text
Bearer {{token}}
```

Expected status:

```text
200 OK
```

---

## Negative Tests

Examples of negative tests:

- Registration with empty fields
- Registration with incorrect email format
- Login with wrong email
- Login with wrong password
- Add car without authorization
- Add car with wrong token
- Add car with invalid year
- Add car with invalid seats

---

## Status Codes

| Status Code | Meaning |
|---|---|
| 200 | Successful request |
| 400 | Validation error or incorrect request data |
| 401 | Unauthorized request |
| 403 | Forbidden request |
| 500 | Server error |

---

## Notes

Protected Car Controller requests require a valid token from Login.

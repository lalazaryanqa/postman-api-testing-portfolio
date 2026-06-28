# Phonebook / Contact App API Postman Tests

This folder contains Postman API tests for the Phonebook / Contact App backend application.

## Swagger

```text
https://contactapp-telran-backend.herokuapp.com/swagger-ui/index.html
```

## Base URL

```text
https://contactapp-telran-backend.herokuapp.com
```

## Tested Controllers

- Authentication Controller
- Contact Controller

---

## Collection Variables

```text
BaseURL_Phonebook
email
password
token
contactId
```

## Authentication Flow

### Registration

Creates a new user.

```text
POST {{BaseURL_Phonebook}}/v1/user/registration/usernamepassword
```

Example body:

```json
{
  "username": "{{$randomEmail}}",
  "password": "Password123!"
}
```

Expected status:

```text
200 OK
```

Important note: Phonebook registration accepts only `username` and `password`.  
Fields such as `firstName` and `lastName` should not be sent in this request.

The registration script saves:

```text
email
password
```

---

### Login

Logs in with the registered user and saves JWT token.

```text
POST {{BaseURL_Phonebook}}/v1/user/login/usernamepassword
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

## Contact Controller Tests

### Add Contact

Creates a new contact.

```text
POST {{BaseURL_Phonebook}}/v1/contacts
```

Authorization:

```text
Bearer {{token}}
```

Example body:

```json
{
  "name": "{{$randomFirstName}}",
  "lastName": "{{$randomLastName}}",
  "email": "{{$randomEmail}}",
  "phone": "1234567890",
  "address": "Haifa",
  "description": "test contact"
}
```

Expected status:

```text
200 OK
```

Expected response:

```json
{
  "message": "Contact was added! ID: ..."
}
```

The script saves the generated contact ID into:

```text
contactId
```

---

### Get All Contacts

Gets all contacts of the current user.

```text
GET {{BaseURL_Phonebook}}/v1/contacts
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

### Update Contact

Updates an existing contact.

```text
PUT {{BaseURL_Phonebook}}/v1/contacts
```

Authorization:

```text
Bearer {{token}}
```

Example body:

```json
{
  "id": "{{contactId}}",
  "name": "{{$randomFirstName}}",
  "lastName": "{{$randomLastName}}",
  "email": "{{$randomEmail}}",
  "phone": "1234567890",
  "address": "Tel Aviv",
  "description": "Updated contact"
}
```

Expected status:

```text
200 OK
```

Expected response:

```json
{
  "message": "Contact was updated"
}
```

---

### Delete Contact

Deletes a contact by ID.

```text
DELETE {{BaseURL_Phonebook}}/v1/contacts/{{contactId}}
```

Authorization:

```text
Bearer {{token}}
```

Expected status:

```text
200 OK
```

If the same contact is deleted twice, the server may return `400 Bad Request` because the contact no longer exists.

---

## Negative Tests

### Add Contact Without Authorization

```text
POST {{BaseURL_Phonebook}}/v1/contacts
```

Authorization:

```text
No Auth
```

Expected status:

```text
403 Forbidden
```

---

### Get All Contacts Without Authorization

```text
GET {{BaseURL_Phonebook}}/v1/contacts
```

Authorization:

```text
No Auth
```

Expected status:

```text
403 Forbidden
```

---

## Status Codes

| Status Code | Meaning |
|---|---|
| 200 | Successful request |
| 400 | Bad request, invalid data, or object not found |
| 401 | Unauthorized, incorrect login/password or invalid token |
| 403 | Forbidden, request without required authorization |

---

## Notes

- PATCH was not added because it is not available in Swagger documentation.
- Before running Contact Controller requests, Login should be executed to generate a valid token.
- If token disappears after reopening Postman, run Login again.

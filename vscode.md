# VS CODE

- [Plugins](#pluginsextensions)

## Plugins/Extensions

- [Rest client](#rest-client)
- [SQLITE](#sqlite)

### Rest client

Install the **rest client** extension.
Make http requests from a file directly in VSCode.  

Create a file : **requests.http**

Example file:

```text
### Get one user by id
GET http://localhost:3000/auth/3

### Create user
POST http://localhost:3000/auth/signup
content-type: application/json

  # "email": "sheldon@gmail.com",
{
  "email": "panay@gmail.com",
  "password":"bitch"
}

### Get all user by email
GET http://localhost:3000/auth?email=rico@gmail.com 

### Get all users
GET http://localhost:3000/auth

### Update password with id
PATCH  http://localhost:3000/auth/1
content-type: application/json

{
  "id":"1",
  "email": "rico@gmail.com",
  "password": "some more chaos"
}

### Remove user with given id
DELETE http://localhost:3000/auth/3
```

/!\ On **LINUX** install the sqlite package:  
`sudo apt install sqlite`

Type **Ctrl+p** to open the VSCode settings.  
Type **SQLite: open database** and select it.  
If an _http_ file exists in the project then VSCode will list it.
A new dropdown should appear in the EXPLORER panel of VSCode called: **SQLITE EXPLORER**

### SQLITE

Install the **SQLite** extension.

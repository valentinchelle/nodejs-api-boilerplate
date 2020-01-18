# REST API Tutorial

This sample is published as part of the blog article at www.toptal.com/blog:

https://github.com/makinhs/rest-api-tutorial

- https://www.toptal.com/nodejs/secure-rest-api-in-nodejs

Visit www.toptal.com/blog and subscribe to our newsletter to read great posts

## Before using

- Please make sure that you have:
- node.js installed (https://nodejs.org/)
- have mongodb installed and running locally (https://www.mongodb.com/)
  - Using Windows, just open the terminal at where you installed mongo and run `mongod.exe`
- run npm install in your root project folder

## Usage

To run the project, please use a command line the following:

- npm start

  - It will run the server at port 3600.

- mongodb configuration in common/services/mongoose.service

- Add a file `.env` at the root of the folder (same level as `index.js`), copy paste the following template and fill it with the right information like :

```
PORT=3600
APP_END_POINT=http://localhost:3600
API_END_POINT=http://localhost:3600
JWT_SECRET=r4ndomStr1ng
JWT_EXPIRATION_IN_SECONDS=36000
```

## Api Endpoints

### `localhost:3600/users` : Insert a new user

Request Body :

```
{
	"firstname": "Valentin",
	"lastname": "Rigolo",
	"email": "valentin.rigolo@azerty.com",
	"password": "secret"

}
```

# Understanding the login logic

## Step 1 : Login with credentials

The first step in the login process, is for a registered user to send its email/passsword.
Endpoint : `endpoint:3600/auth`
Body :

```
{
	"email": "user@azerty.com",
	"password": "secret"
}
```

If the login credentials are right, the response should be like:

```
{
"accessToken": "eyJhbGcidOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI1ZTIzNjRlZDQ4NDE2YzY2ZjYwMGIwYjgiLCJlbWFpbCI6InZhbGVudGluLmNoZWxsZTNAZ21haWwuY29tIiwicGVybWlzc2lvbkxldmVsIjoxLCJwcm92aWRlciI6ImVtYWlsIiwibmFtZSI6InVuZGVmaW5lZCB1bmRlZmluZWQiLCJyZWZyZXNoS2V5IjoiRUlVUk12NENQKytOZitzSHRnZThFZz09IiwiaWF0IjoxNTc5Mzc5NTY1fQ.jBIa5DNI0ObjVW7i3qF68XguKxuw_4lLmr-5S15rOp4",
"refreshToken": "RFZJZE5YRWldTS3B2bWtUUlByaVZUeVJNTVFyYko5Sm80OUsycWtYUU9PYlFuQURCYkh4K202YWxnd2IybmFiRkQ0TWl2TElQemJKeGUrQ3FpdXVmR3c9PQ=="
}
```

The access token is the famous `json web token`. It is a stateless string that will allow someone to stay logged everytime they provide this token to the server.

## Step 2 : Stay logged with the JWT

In order to make your users stay logged on your app, you have to provide the jwt for each request. To do that,
Grab the `accessToken` from the previous request, prefix it with `Bearer` and add it to the request headers under Authorization.
This will be required for every route with the middleware `ValidationMiddleware.validJWTNeeded` like :

```
  app.patch("/users/:userId", [
    ValidationMiddleware.validJWTNeeded,
    PermissionMiddleware.minimumPermissionLevelRequired(FREE),
    PermissionMiddleware.onlySameUserOrAdminCanDoThisAction,
    UsersController.patchById
  ]);
```

# References

Encrypting the passwords :
https://github.com/goldbergyoni/nodebestpractices/tree/security-best-practices-section#-68-avoid-using-the-nodejs-crypto-library-for-passwords-use-bcrypt

### 2019-09-13 update

- Refactored mongoose to a proper common service.
- Added a Dockerfile and docker-compose configuration.

If you are familiar to docker and you have docker installed on your machine and just want to run the project without issues please do:

- docker-compose build
- docker-compose up
- It will run the mongodb at port 27017 (for testing purposes only).
- It will run the server at port 3600.

```

```

```

```

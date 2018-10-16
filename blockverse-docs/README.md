# blockverse-docs
# Blockverse API authentication
The tutorial lays out the workflow that a user would follow for authenticating and making API calls via postman. In order to get postman, please visit:

https://www.getpostman.com/

## Obtaining JWT token
The first step before obtaining the JWT token is to ensure that 2FA is set up.
To set up 2FA please visit blockverse.io/security after verifying phone and identity.
Once 2FA is set up, make the following request:

```ruby
POST http://blockverse.io:3030/api/v1/sessions
payload = { 
  "email":  "test@blockverse.io", # Use your email
  "password": "test1234", # Use your password 
  "application_id": "b224ad08ff71e3d8731c44e2cc529cac119659229ab7ef3a4b9c7daa43320bd1", #Use this application id
  "otp_code": "123123" # Use your 2FA code
}
```
For postman, using the POST prefix, and the url specified above, input the payload in the body section. Use raw JSON data type, as shown in the screen shot below. Ensure that the payload is formatted the same way as in the screenshot, with no comments included (will throw errors)
```ruby
{
	"email": "sample@sample.io",
	"password": "sample",
	"application_id": "b224ad08ff71e3d8731c44e2cc529cac119659229ab7ef3a4b9c7daa43320bd1",
	"otp_code": "123123"
}
```

![alt text](https://i.imgur.com/hmhNj48.png)
This will return a JWT token. You can now make api requests. 

### Restful API

### Documentation
To see the api documentation, please visit http://blockverse.io/swagger
Once on the swagger page, input http://blockverse.io/api/v2/swagger into the url field (will be prepoulated with a similar link), and the JWT token from above, and press explore. The response should be similar to this

![alt text](https://i.imgur.com/FdyyBab.png)

### Authentication
For all api requests, use Bearer Token authentication with the jwt token obtained from the api request above

**Example** of header in a restful api request:

```JSON
{
  "Authorization": "Bearer <Token>"
}
```

### Websocket API

### Authentication

Authentication happens on websocket message with following JSON structure.

```JSON
{
  "jwt": "Bearer <Token>"
}
```

If authenticaton was done, server will respond successfully

```JSON
{
  "success": {
    "message": "Authenticated."
  }
}
```

Otherwise server will return an error

```JSON
{
  "error": {
    "message": "Authentication failed."
  }
}
```

If authentication JWT token has invalid type, server return an error

```JSON
{
  "error": {
    "message": "Token type is not provided or invalid."
  }
}
```

If other error occured during the message handling server throws an error

```JSON
{
  "error": {
    "message": "Error while handling message."
  }
}
```

**Note:** Blockverse websocket API supports authentication only Bearer type of JWT token.

**Example** of authentication message:

```JSON
{
  "jwt": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
}
```

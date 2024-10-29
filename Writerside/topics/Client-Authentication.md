# Client Authentication 

This page describes the different methods for authenticating a client.


## Specifications

- [OAuth 2.1 draft](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-11).
- [OAuth Assertion](https://www.rfc-editor.org/rfc/rfc7521.html).


## Authentication

There are different methods for authenticating the client.
Authentication can be classified in two categories, which is client secret or public/private key cryptography.


### Client Secret Basic

Passing the client_id and client_secret in the Authorization header.
The credentials must be url encoded and finally base64 encoded.

For example if the client_id has value "identifier" and client_secret has value "secret".
The combined header before base64 encoding is "identifier:secret".
After base64 encoding it becomes "aWRlbnRpZmllcjpzZWNyZXQ".

```http request
POST /protected-endpoint HTTP/1.1
Host: idp.authserver.dk
Content-Type: application/x-www-form-urlencoded
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

argument1=value1&argument2=value2
```


### Client Secret Post

Passing the client_id and client_secret in the body.

```http request
POST /protected-endpoint HTTP/1.1
Host: idp.authserver.dk
Content-Type: application/x-www-form-urlencoded

client_id=identifier&client_secret=secret
```


### Private Key JWT

Passing an assertion as a JWT, which is signed using a private key.
AuthServer then verifies the signature, through the registered jwks from the client metadata.

An example assertion can be here, which is passed to the token endpoint:

```json lines
{
  "alg": "RS256",
  "kid": "0610750f-683e-4118-9b7b-de85456fdfe9",
  "typ": "pk+jwt"
}
{
  "aud": "https://idp.authserver.dk/connect/token",
  "exp": "1730239956",
  "iat": "1730239856",
  "iss": "d3e1ab02-66ec-469b-b53d-11e7ed772c59",
  "nbf": "1730239856"
}
```

```http request
POST /protected-endpoint HTTP/1.1
Host: idp.authserver.dk
Content-Type: application/x-www-form-urlencoded

client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3A
client-assertion-type%3Ajwt-bearer&
client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0...
```
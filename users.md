## Users API

### Register a new user

The only required parameters to create a user are email and password. But other
parameters such as `first_name`, `last_name` and `role` are also supported.
Request: `POST /v1/users`
```json
{
  "email": "killa@gorilla.com",
  "password": "topsecret"
}
```

Response (for request w/o company in request):
```json
{
  "id": 2,
  "email": "killa@gorilla.com",
  "first_name": null,
  "last_name": null,
  "role": "owner",
  "status": "active",
  "company": {
    "name": null,
    "url": null,
    "logo_url": null,
    "landing_domain": null,
    "client_id": "chaXxKnsZJ"
  },
  "auth": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM",
    "token_type": "bearer",
    "expires_in": 3600,
  }
}
```

Use `auth.access_token` from the response to [authenticate](./authentication.md)
all other requests to API.

If you send request w/o `Authorization` HTTP header, user will be created with
a new company. And this user will be company's owner in spite of `role`'s value
from request. So you can add a company section to request in order to specify
company's parameters (the most important is `landing_domain`, it is required to
prepare gift's `share_url` and serve it):
`POST /v1/users`
```json
{
  "email": "killa@gorilla.com",
  "password": "topsecret",
  "company": {
    "name": "Roga & Kopyta",
    "url": "http://rogakopyta.co.uk",
    "logo_url": "//www.google.com/logos/doodles/2015/42nd-anniversary-of-the-official-recognition-of-the-letter-5644871192805376.2-hp2x.gif",
    "landing_domain": "rk.swiftgift.me"
  }
}
```

But if you send request with `Authorization` header, a new user will be created
as a part of your current user's company. In this case you can set role of
a new user: `owner` (can do everything), `user` (can log in to website,
but cannot change anything there), `admin` is the same as `user`, but also can
create gifts via API.

### Log user in

Request: `POST /v1/users/login`
```json
{
  "email": "killa@gorilla.com",
  "password": "topsecret"
}
```

Response:
```json
{
  "id": 2,
  "email": "killa@gorilla.com",
  "first_name": null,
  "last_name": null,
  "role": "owner",
  "status": "active",
  "company": {
    "name": "Roga & Kopyta",
    "url": "http://rogakopyta.co.uk",
    "logo_url": "https://www.google.com/logos/doodles/2015/42nd-anniversary-of-the-official-recognition-of-the-letter-5644871192805376.2-hp2x.gif",
    "landing_domain": "rk.swiftgift.me",
    "client_id": "chaXxKnsZJ"
  },
  "access_token": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM",
    "token_type": "bearer",
    "expires_in": 3600,
  }
}
```

Use `auth.access_token` from the response to [authenticate](./authentication.md)
all other requests to API.

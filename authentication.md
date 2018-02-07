## Authentication

### By access token

The most of API resources require passing OAuth2-style access token in `Authorization` HTTP header with `Bearer ` prefix, for example:
```
POST /v1/gifts
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM
{
  ...
}
```

Access token can be obtained by two ways: [User login API](./users.md#login) or OAuth2's "Client Credentials grant" flow.

To obtain token by Client Credentials grant flow, you need to know Client ID and Client Secret from your account and put them into request:
```
POST /v1/oauth

grant_type=client_credentials&client_id=<ID>&client_secret=<SECRET>
```
Response:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM",
  "token_type": "bearer",
  "expires_in": 3600,
}
```

### By Client ID

Also we use another way of authentication for API resources which are requested from the apps implemented and working on front end side (browsers) only. These resources do not contain any  confidential information and we use a public Oauth2 client key there. Currently we have only one resource which uses this authentication method:
```
POST /v1/gifts/public/<cliendId>
{
  ...
}
```

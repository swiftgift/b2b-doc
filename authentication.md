## Authentication in SwiftGift API

### Access token

The most of API resources require passing OAuth2-style access token in `Authorization` HTTP header with `Bearer ` prefix, for example:
```
POST https://api.swiftgift.me/v1/gifts
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM

{
  "data": "123"
}
```

Access token can be obtained by two ways: [User login API](./users.md#login) or OAuth2's "Client Credentials grant" flow.

To obtain token by Client Credentials grant flow, you need to know Client ID and Client Secret from your account and put them into request:
```
POST https://api.swiftgift.me/v1/oauth

grant_type=client_credentials&client_id=<ID>&client_secret=<SECRET>
```
Response:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM",
  "token_type": "bearer",
  "expires_in": 3600,
  "refresh_token": "FFDAh2UclWHV9SNAF8iI1BSxUWlgUKmoLBOHE04EZD2Ipi6jjPW5im83iIkvoBJIaLNHWWsCrlXB6cWteEFTx2bLaN1kOgLgj5SJeSs1QATdXCmwTwVw3P6RC7e054nQWiUAXSG1nzl3Qc11aHI2xaTj3gAwjzLroN8IHSuRzQk7amU0oHJhydozS1mFAQuKEp4Muo8cxD7TuWvy6kteOqfyStstGHGSXcVn3TdsGlvmmmTQj1qNXfJV9rkWFF3"
}
```

### Refresh access token

Each access token has limited lifetime. To refresh an access token send the request with `refresh_token`:
```
POST https://api.swiftgift.me/v1/oauth
Authorization: Bearer <expired_access_token>

grant_type=refresh_token&refresh_token=<refresh_token>
```

There are two approaches to refresh stored access tokens:
  - catch `auth_expired` error on any request to API, refresh token and re-try to perform an original request.
  - regularly check for tokens which are going to expire soon and refresh them.

The first way may cause race conditions, when two or more parallel processes will try to refresh the same token.

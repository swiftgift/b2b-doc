## Authentication

All requests to API should contain credentials in order to identify what partner these requests are sent by.

The most of private API resources require passing Oauth2 access token in `Authorization` HTTP header with `Bearer ` prefix, for example:
```
POST /api/v1/stripe_card
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOjEsImV4cCI6MzMwNTIzNjYxMDQsInZlciI6MSwiaWF0IjoxNTE2MzY2MTA0fQ.M6vNPa9yG19ez3xrej4MBk9slmhjYqlBJcbC8RkcQcM
```

Also we use another way of authentication for API resources which are requested from the apps implemented and working on front end side (users' browsers) only. These resources do not contain any  confidential information and we use a public Oauth2 client key there. For example:
```
POST /v1/shopify/gifts/<cliendId>
{
  ...
}
```

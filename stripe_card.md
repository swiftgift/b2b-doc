## Stripe cards API

All Stripe cards' resources requires authentication by passing Oauth2 [access
token](./authentication.md).

### Store a new card

Request:
```json
POST /api/v1/stripe_card
{
  "token": "<Stripe source token obtained via Stripe.js>"
}
```

Response:
```json
{
  "id": "card_1BlwBNB56EWzwckaPKhlvR4Z",
  "object": "card",
  "address_city": null,
  "address_country": null,
  "address_line1": null,
  "address_line1_check": null,
  "address_line2": null,
  "address_state": null,
  "address_zip": "54321",
  "address_zip_check": "pass",
  "brand": "Visa",
  "country": "US",
  "customer": "cus_CAGjsELuona4Zq",
  "cvc_check": "pass",
  "dynamic_last4": null,
  "exp_month": 11,
  "exp_year": 2019,
  "fingerprint": "9XzxVLAnPhmE9k6f",
  "funding": "credit",
  "last4": "4242",
  "metadata": {},
  "name": null,
  "tokenization_method": null
}
```

### Delete card

Request: `DELETE /api/v1/stripe_card`
Successful response has 204 HTTP status code and empty body.

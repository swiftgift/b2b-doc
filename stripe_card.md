## Stripe cards API

This API resource requires authentication by [access token](./authentication.md).

### Store a new card

Allows to store user's card to pay for service automatically at the end of each
month (billing period). Payments are provided by [Stripe](https://stripe.com/),
so you can use [Stripe.js](https://stripe.com/docs/stripe-js/reference) or
another client library to obtain card's source token.

Request: `POST /v1/stripe_card`
```json
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

### Delete a current card

Request: `DELETE /v1/stripe_card`
Successful response has 204 HTTP status code and empty body.

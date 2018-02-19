## Statistics

This API resource requires authentication by [access token](./authentication.md).

The resource provides statistics about gifts for some period of time (by
default it's a current month).

Request: `GET /v1/stat`
```json
{
  "gifts": {
    "total": 2,
    "accepted": 1,
    "earned": "15.70",
    "fee": "0.24"
  },
  "free_till": "2018-03-05T07:17:38.397Z",
  "card_error": null
}
```

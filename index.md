## SwiftGift API spec

### Authentication
You should add authorization header with your token to each HTTP request requires authentication:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJhMSIsImV4cCI6MTQ4NzY3MTAxNDYyOCwiaWF0IjoxNDg3NjcwOTU0fQ.mC5oW75uBMn1w_4M7Sws1n5apDY8O3lGblIS1I_OE7c
```

### Configuring client

TODO:

### Gifts

#### Create gift
When your user wants to send order as a gift, you create a new gift via our API.

Request:
```json
POST https://api.swiftgift.me/v1/gifts/<client_id>
{
  "idempotency_key": "8557f5f0-f858-410a-a75e-68e2a008c84a",
  "sender": {
    "first_name": "Sherlock",
    "last_name": "Holmes",
    "image_url": "//cdn-staging.swiftgift.me/images/users/8c/2baa6eda270623f03c4d8d44bec9d2.jpg", // Optional
    "email": "sherlock@holmes.co.uk",
    "phone_number": "" // Optional
  },
  "message": { // Optional
    "text": "Happy birthday, my dear Watson!",
    "image_url": "//cdn-staging.swiftgift.me/images/users/a2/7a5bd5a0f9943fc021adae269c316b.jpg"
  },
  "sender_ip": "212.58.244.23", // Optional your user's IP address to exclude their visits to share URL (viewed field).
  "is_surprise": false,
  "products": [ // Not needed if is_surprise=true
    {
      "name": "Aberfeldy - Highland Single Malt Scotch (12 Year Old)",
      "image_url": "//cdn.swiftgift.me/images/products/5a/842ba3d3f2341e6a5a0c49c0735699.jpg"
    },
    {
      "name": "Mystery Fidget Spinners",
      "image_url": "https://cdn.swiftgift.me/images/products/0a/9b856f95e0afc3a3fbc16276314b13.jpg"
    }
  ],
  "basket_amount": "45.23",
  "currency": "GBP",
  "delivery": {
    "country": "GB", // Required! It is
    "name": "DHL Standard Delivery", // Optional
    "min_time": 1,
    "max_time": 2
  },
  "callback_url": "https://mysite.com/orders/123/callback", // Optional if you will poll our API for changes
  "reminders": [24, 48] // Optional, we will notify your user by email if the recipient will not react (accept or reject)
}
```
Response:
```json
{
  "id": 321,
  "status": "pending",
  "share_url": "https://gifts.yoursite.com/al0z-kL8fe", // Or https://yoursite.swiftgift.me/al0z-kL8fe if you can't create 3rd level domain which points to our IP.
  "delivery_address": {},
  "reply": {
    "text": "",
    "image_url": ""
  },
  "reminded": null,
  "viewed": null,
  "accepted": null,
  "dispatched": null,
  "replied": null,
  "updated": "2017-02-11T08:51:08.522Z",
  "created": "2017-02-11T08:51:08.522Z"
}
```
After gift is created, your user should send `share_url` to the recipient using any channel they prefer (SwiftGift URL will come with a nice preview in Whatsapp, Facebook Messenger, Viber, Telegram, iMessage and Android).
URL takes recipient to Gift Receive Landing Page, which will look like your site and will be customisable via admin panel.
Gift Receive Landing Page contains: gift description (if `is_surprise=false`); sender’s message and photo (optional); and scheduled delivery date.

Recipient provides delivery address, and clicks “accept”.
After acceptance, Recipient has capability to attach a “Thank You” message (text + image). We provide white label template for Thank You Message Page, and will distribute the message to Sender via email.

After sender shares URL, we wait for recipient’s decision (with capability to send reminders to the sender).
If Recipient opens or accepts the gift, your API will be called back with changes.

```sh
curl -d'{"idempotency_key": "123", "sender": {"first_name": "Sherlock", "last_name": "Holmes", "image_url": "//cdn-staging.swiftgift.me/images/users/8c/2baa6eda270623f03c4d8d44bec9d2.jpg", "email": "sherlock@holmes.co.uk", "phone_number": ""}, "message": {"text": "Happy birthday, my dear Watson!", "image_url": "//cdn-staging.swiftgift.me/images/users/a2/7a5bd5a0f9943fc021adae269c316b.jpg"}, "sender_ip": "212.58.244.23", "is_surprise": false, "products": [{"name": "Aberfeldy - Highland Single Malt Scotch (12 Year Old)", "image_url": "//cdn.swiftgift.me/images/products/5a/842ba3d3f2341e6a5a0c49c0735699.jpg"}, {"name": "Mystery Fidget Spinners", "image_url": "https://cdn.swiftgift.me/images/products/0a/9b856f95e0afc3a3fbc16276314b13.jpg"}], "basket_amount": "45.23", "currency": "GBP", "delivery": {"country": "GB", "name": "DHL Standard Delivery", "min_time": 1, "max_time": 2}, "callback_url": "https://mysite.com/orders/123/callback", "reminders": [24, 48]}' 'https://api-staging.swiftgift.me/v1/gifts/c123'
```

### Get gift (polling model)
This can be useful if you don't want to use Callback URL and want to poll changes on your side.

Request: `GET https://api.swiftgift.me/gifts/123`
Response (example: where gift has been accepted):
```json
{
  "id": 321,
  "status": "accepted",
  "delivery_address": {
    "country": "Great Britain",
    "state": "", // Optional for US
    "city": "London",
    "postcode": "NW1 6XE",
    "street_address1": "Baker Street",
    "street_address2": "221B",
    "first_name": "John",
    "last_name": "Watson",
    "phone_number": "+44 20 7224 3688"
  },
  "reply": {
    "text": "Thanks, dear Holmes!",
    "image_url": "https://bunkstrutts.files.wordpress.com/2012/05/kraken-crackin.gif"
  },
  "reminded": null,
  "viewed": "2017-02-11T15:32:43.103Z",
  "accepted": "2017-02-11T15:35:36.637Z",
  "dispatched": null,
  "replied": "2017-02-11T15:35:36.637Z",
  "updated": "2017-02-11T15:35:36.637Z",
  "created": "2017-02-11T08:51:08.522Z"
}
```

### Get gift via callback (pushing model)
If you have provided a `callback_url`, every time there is a change to Gift we call your API with updated details.

Request (example: where gift has been viewed, but not yet accepted):
```json
POST https://mysite.com/orders/123/callback
{
  "id": 321,
  "status": "accepted",
  "delivery_address": {},
  "reply": {
    "text": "",
    "image_url": ""
  },
  "reminded": null,
  "viewed": "2017-02-11T15:32:43.103Z",
  "accepted": null,
  "dispatched": null,
  "replied": null,
  "updated": "2017-02-11T15:32:43.103Z",
  "created": "2017-02-11T08:51:08.522Z"
}
```

### Update gift
This is not a mandatory step, but if you update us when gift has been dispatched - we can notify the Recipient via email, and also update the Gift Receive Landing Page.

Request:
```json
POST https://api.swiftgift.me/gifts/123
{
  "status": "dispatched",
  "dispatched": "2017-02-11T16:03:25.957Z"
}
```

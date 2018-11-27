## SwiftGift B2B gifting API

Root URL of API is https://api.swiftgift.me/ or https://api-staging.swiftgift.me for testing.

### Integration flow

1. Your customer sees "Send as Gift" option on checkout (+ product's page), which does not require to specify delivery address immediately.

2. Customer makes a purchase.

3. You creates a gift by [request](./gifts.md#create-gift) to SwiftGift API and display `share_url` from the response to your customer.

4. Customer sends this URL to the recipient.

5. Recipient accepts the gift by providing their delivery address.

6. We make a request to your `callback_url` provided on step #3 with recipient's delivery address in the body. If you do not want to implement serving `callback_url` on your side, then you have to request the gift via [API](./gifts.md#get-gift) regularly to check changes of gift's `status` and `delivery_address`.

### Reference

* [Authentication](./authentication.md)
* [Gifts](./gifts.md)
* [Users](./users.md)
* [Statistics](./stat.md)
* Clients:
  * [TODO] [Node.js](./clients/nodejs)
  * [TODO] [Python](./clients/nodejs)
* Plugins:
  * [Shopify](https://splugin.swiftgift.me/)
  * [Magento 1](https://marketplace.magento.com/swiftgift-sg-gift.html), [source code](https://github.com/swiftgift/magento1-module)
  * [Magento 2](https://marketplace.magento.com/swiftgift-gift.html), [source code](https://github.com/swiftgift/magento2-module)

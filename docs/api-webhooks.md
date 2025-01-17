---
id: api-webhooks
title: Webhooks
---

When one of your users makes a payment, you need to know the status of that payment. The payment goes through several different status in its lifetime. When the status of a payment changes, we ping your webhook URL.

The status of a payment can be one of these things:

| name        | description                                   |
|-------------|-----------------------------------------------|
| `PENDING`   | tx has been built but not yet signed          |
| `RECEIVED`  | tx has been signed, broadcasted, and is valid |
| `COMPLETED` | tx has been confirmed in a block              |
| `FAILED`    | tx has been rejected by the network           |

Your webhook URL must accept a POST request with JSON. The object we send you looks like this:

```
{ secret, payment }
```

The `payment` object looks like this:

| name             | type     |
|------------------|----------|
| `id`             | `string` |
| `buttonId`       | `string` |
| `buttonData`     | `string` |
| `status`         | `string` |
| `txid`           | `string` |
| `normalizedTxid` | `string` |
| `amount`         | `string` |
| `currency`       | `string` |
| `satoshis`       | `string` |
| `outputs`        | `array`  |

Note that merely receiving a webhook does not necessarily indicate a successful payment. The `payment` object has a `status`, and `status` can be `DOUBLE_SPENT` indicating an earlier payment is no longer valid.

Where the outputs look like this:

| name       | type                                       |
| ---------- | ------------------------------------------ |
| `type`     | `string` (`'address', 'userId', 'script'`) |
| `address`  | `string`                                   |
| `userId`   | `string`                                   |
| `script`   | `string`                                   |
| `amount`   | `string`                                   |
| `currency` | `string`                                   |
| `satoshis` | `number`                                   |

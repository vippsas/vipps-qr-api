# Vipps Vipps QR API v1

💥 DRAFT: This is unfinished work and subject to change. 💥

The Vipps QR API lets merchants generate Vipps QR codes that can be used to pay
over the counter, without requiring the Vipps user to provide their telephone
number to the merchant.

Every Vipps payment needs a unique `orderId`, so it's not possible to make
one static QR code, like a sticker or a poster. Each payment must have a
unique QR code. That is what the Vipps QR API is for.

The QR code, when scanned, will take the customer straight to the payment
screen in the Vipps app.

API version: 1.0.0.

Document version 1.1.1.

# Table of contents

- [Basic flow](#basic-flow)
  * [Before you begin](#before-you-begin)
  * [Authentication](#authentication)
- [Initiate a payment with the Vipps eCom API](#initiate-a-payment-with-the-vipps-ecom-api)
- [Generate the QR code](#generate-the-qr-code)
  * [Request](#request)
  * [Response](#response)
- [QR formats](#qr-formats)
  * [How to specify the QR format](#how-to-specify-the-qr-format)
- [Questions?](#questions-)

# Basic flow

1. Initiate a Vipps eCom payment
2. Receive the payment URL as response
3. Post the payment URL to the QR API
4. Receive a URL to a QR code i PNG (Portable Network Graphics) format

## Before you begin

This document assumes you have signed up as a organisation with Vipps and have
retrieved your API credentials for
[the Vipps test environment](https://github.com/vippsas/vipps-developers/blob/master/vipps-test-environment.md)
from
[portal.vipps.no](https://portal.vipps.no).

## Authentication

All Vipps API calls are authenticated and authorized with an access token
(JWT bearer token) and an API subscription key:

| Header Name | Header Value | Description |
| ----------- | ------------ | ----------- |
| `Authorization` | `Bearer <JWT access token>` | Type: Authorization token. This obtained as described in [Getting started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md): [Get an access token](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#get-an-access-token) |
| `Ocp-Apim-Subscription-Key` | Base 64 encoded string | The subscription key for this API. This is available on [portal.vipps.no](https://portal.vipps.no). |

For more information about how to obtain an access token and all details around this, please see:
[Quick overview of how to make an API call](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#quick-overview-of-how-to-make-an-api-call)
in the
[Getting started guide](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md).

# Initiate a payment with the Vipps eCom API

Before creating the QR code you must initiate a payment with the Vipps eCom API as described in the
[eCom API guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate-payment-flow-phone-and-browser).

# Generate the QR code

The
[`POST:/ecomm/v2/payments`](https://vippsas.github.io/vipps-ecom-api/#/Vipps%20eCom%20API/initiatePaymentV3UsingPOST)
endpoint will return a response like this (the `url` is truncated, but the format is correct):

```json
{
  "orderId": "acme-shop-123-order123abc",
  "url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"
}
```

Be aware that the URL is only valid for a short period of time. See the
[eCom API guide: Timeouts](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#timeouts)
for details.

## Request

Now that you have the `url` from the Vipps eCom API you can create a QR code
using the following endpoint:

[`POST:qr​/v1/`](https://vippsas.github.io/vipps-qr-api/#/QR/generateQr)

Example:

Header:
```
POST https://vippsas.github.io/vipps-qr-api/#/QR/generateQr
client_id: fb492b5e-7907-4d83-ba20-c7fb60ca35de
client_secret: Y8Kteew6GE2ZmeycEt6egg==
Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a
Vipps-System-Name: Acme Enterprises Ecommerce DeLuxe
Vipps-System-Version: 3.1.2
Vipps-System-Plugin-Name: Point Of Sale Excellence
Vipps-System-Plugin-Version 4.5.6
```

Body:
```json
{
  "url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"
}
```

## Response

The response will be similar to this:

```json
{
  "url":"https://qr.vipps.no/generate/qr.png?uri=https://short.vipps.no/v1/url?id=01660693bd8f4311a47ffe4c823fb42a&qr-only=true",
  "expiresIn":60
}
```

**Please note:** The `expiresIn` value is in seconds.

# QR formats

For the most normal usecase, the `qr.vipps.no/generate/qr.png <snip>` URL will
generate a QR code and return a `https://short.vipps.no` URL with a link to an image.

The QR code image is available at the URL in the response:
https://short.vipps.no/v1/url?id=01660693bd8f4311a47ffe4c823fb42a&qr-only=true

It is also possible to only get the `targetUrlcode` of the QR if you want to
generate the QR yourselves by using the appropriate accept header.
This will require an approval from Vipps before it is used, so we can validate
the styling of the QR.

The `targetUrl` that points to `https://short.vipps.no` is a shortened URL
that will redirect to the payment URL that was posted to the API.
Since the `targetUrl` is quite short, the generated QR code is less complex and
easier to scan efficiently.

## How to specify the QR format

Use the following HTTP Accept headers:

Accept Headers   | Return type  | Example
------------   | ------------- | --------
`Accept: image/png`      | Will return a URI to an PNG image | https://qr.vipps.no/generate/qr.png
`Accept: image/*`        | Will return a URI to an PNG image | https://qr.vipps.no/generate/qr.png
`Accept: text/targetUrl` | Will return the target URL        | https://short.vipps.no/url/v1?id=Ab4c7

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

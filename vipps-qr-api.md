# Vipps Vipps QR API v1

💥 DRAFT: This is unfinished work and subject to change. 💥

The Vipps QR API lets merchants generate Vipps QR codes that can be used to pay
over the counter, without requiring the Vipps user to provide their telephone
number to the merchant.

The QR code, when scanned and opened, will redirect the user to the Vipps
landing page which on the phone will automatically trigger a switch to the
Vipps app where they can pay the merchant.

API version: 1.0.0.

Document version 1.0.2.

# Table of contents

- [Basic flow](#basic-flow)
- [Getting Started](#getting-started)
  * [Before you begin](#before-you-begin)
  * [Get an access token](#get-an-access-token)
  * [Generate the QR code](#generate-the-qr-code)
- [API summary](#api-summary)
- [Questions?](#questions-)

# Basic flow

1. Initiate a Vipps eCom payment
2. Receive the payment URL as response
3. Post the payment URL to the QR API
4. Receive a URL to a QR code i PNG (Portable Network Graphics) format

# Getting Started

## Before you begin

This section covers the quick steps for getting started with the Vipps QR API.
This document assumes you have signed up as a organisation with Vipps and have
retrieved your API credentials for
[the Vipps test environment](https://github.com/vippsas/vipps-developers/blob/master/vipps-test-environment.md)
from
[portal.vipps.no](https://portal.vipps.no).

## Get an access token

All Vipps API requests must include an `Authorization` header with
a JSON Web Token (JWT), which we call the _access token_.
The access token is obtained by calling
[`POST:/accesstoken/get`](https://vippsas.github.io/vipps-ecom-api/#/Authorization_Service/fetchAuthorizationTokenUsingPost)
and passing the `client_id`, `client_secret` and `Ocp-Apim-Subscription-Key`.

Request (including the recommended `Vipps-*` HTTP headers):

```
POST https://apitest.vipps.no/accesstoken/get
client_id: fb492b5e-7907-4d83-ba20-c7fb60ca35de
client_secret: Y8Kteew6GE2ZmeycEt6egg==
Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a
```

In response you will get a body and `access_token`, which musrt be used for all
other API requests in the `Authorization` header as the Bearer token.

See more about
[access token](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#get-an-access-token)
in the
[Getting Started guide](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md).

## Generate the QR code

Before creating the QR code an eCom payment needs to be created as described in the
[eCom API guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate-payment-flow-phone-and-browser).

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
[eCom API guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md)
for details.

Afterwards the QR code can be created by using the following endpoint:
[`POST:qr​/v1/`](https://vippsas.github.io/vipps-qr-api/#/QR/generateQr)

HTTP Header   | Value
------------  | -------------
Authorization | Bearer `<accesstoken>`
Accept        | image/png

Body:
```json
{
  "url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"
}
```

Which will generate a response like this:

```json
{
  "url":"https://qr.vipps.no/generate/qr.png?uri=https://short.vipps.no/v1/url?id=01660693bd8f4311a47ffe4c823fb42a&qr-only=true","expiresIn":60
}
```
# QR formats
For the most normal usecase, `qr.vipps.no/generate/qr.png` endpoint will generate a QR code and return a URL with a link to an image. It is also possible to only get the targetUrl of the QR if you want to generate the QR yourselves by using the appropriate accept header. This will however require an approval from us before it is used, so we can validate the styling of the QR.
The targetUrl that points to `short.vipps.no` is a shortened URL that will redirect to the payment URL that was posted to the API.

Accept Headers   | Return type  | Example
------------   | ------------- | --------
image/png      | Will return a URI to an png image | qr.vipps.no/generate/qr.png
image/*        | Will return a URI to an png image | qr.vipps.no/generate/qr.png
text/targetUrl | Will return the target URL        | short.vipps.no/url/v1?id=Ab4c7
# API summary

- [`POST:/qr/v1`](https://vippsas.github.io/vipps-qr-api/#/QR/generateQr)
	- Endpoint for creating a new QR code
- `GET:short.vipps.no/v1/url?id={id}`
	- Shortened URL that will redirect to the payment URL

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

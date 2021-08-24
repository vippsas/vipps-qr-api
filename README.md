# Vipps Vipps QR API v1

💥 This is unfinished work and subject to change. 💥

The Vipps QR API allows merchants to generate Vipps QR codes that can be used to pay over the counter without requiring the Vipps user to provide their telephone number to the merchant. The QR code, when scanned and opened, will redirect the user to the Vipps landing page which on the phone will automatically trigger a switch to the Vipps app where they can pay the merchant.

# Getting Started

## Before you begin

This section covers the quick steps for getting started with the Vipps QR API. This document assumes you have signed up as a organisation with Vipps and have your test credentials from the Merchant Portal.

## 1. Authentication
```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X POST
```

In response you will get a body with the following schema.
The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

```json
{
  "token_type": "Bearer",
  "expires_in": "3599",
  "ext_expires_in": "3599",
  "expires_on": "1614116654",
  "not_before": "1614112754",
  "resource": "00000002-0000-0000-c000-000000000000",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRH <snip>"
  }
}
```

## 2. Creating the QR code

Before creating the QR code an ECOM-payment needs to be created as described [here](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate-payment-flow-phone-and-browser).


The `ecomm/v2/payments` endpoint will return a response like this:

```json
{
  "orderId": "acme-shop-123-order123abc",
  "url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"
}
```

The URL is truncated, but the format is correct. Be aware that the URL is only valid for a short period of time. Consult the ECOM-api docs for details.

Afterwards the QR code can be created by using the following endpoint:
[`POST:qr​/v1/`](https://swagger.io/)

HTTP Header   | Value
------------  | -------------
Authorization | Bearer `<accesstoken>`
Accept        | image/png

Body:
```json
{"url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"}
```

Which will generate a response like this:

```json
{"url":"https://qr.vipps.no/generate/qr.png?uri=https://short.vipps.no/v1/url?id=01660693bd8f4311a47ffe4c823fb42a&qr-only=true","expiresIn":60}
```

The `qr.vipps.no/generate/qr.png` endpoint will generate the QR code as a png with a URL that points to the `short.vipps.no` URL. The `short.vipps.no` URL is a shortened URL that will redirect to the payment URL that was posted to the API.



# Api summary:
- `POST:/qr/v1`
	- Endpoint for creating a new QR code
- `GET:short.vipps.no/v1/url?id={id}`
	- Shortened URL that will redirect to the payment URL

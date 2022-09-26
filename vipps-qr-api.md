<!-- START_METADATA
---
title: API Guide
sidebar_position: 30
---
END_METADATA -->

# Vipps QR API version 1

The Vipps QR API provides you with tools for generating these types of QR codes:

- Merchant redirect - Generate QRs that redirect the user to your website.
- One-time payment - Generate QRs that open the user's Vipps app on their phone and provides the payment suggestion for approval.
  This allows you to initiate a Vipps payment without needing to ask for the customer's telephone number.

Both types of QRs share the same authentication and overall design, but have slight difference in behaviour and how they are made.

API version: 1.2.0.

Document version 1.2.3.

<!-- START_TOC -->

## Table of contents

- [Before you begin](#before-you-begin)
  * [Vipps HTTP headers](#vipps-http-headers)
  * [Authentication](#authentication)
  * [QR formats](#qr-formats)
- [One-Time Payment QR Codes](#one-time-payment-qr-codes)
  * [Basic flow for One-Time Payment QR](#basic-flow-for-one-time-payment-qr)
    - [Initiate a payment with the Vipps eCom API](#initiate-a-payment-with-the-vipps-ecom-api)
    - [Creation of One-Time Payment QR](#creation-of-one-time-payment-qr)
- [Merchant Redirect QR Codes](#merchant-redirect-qr-codes)
  * [Basic flow for Merchant Redirect QR](#basic-flow-for-merchant-redirect-qr)
    - [Creation of Merchant Redirect QR](#creation-of-merchant-redirect-qr)
    - [Updating and Deletion of QRs](#updating-and-deletion-of-qrs)
- [Questions](#questions)

<!-- END_TOC -->

## Before you begin

This document assumes you have signed up as a organisation with Vipps and have
retrieved your API credentials for
[the Vipps test environment](https://github.com/vippsas/vipps-developers/blob/master/vipps-test-environment.md)
from
[portal.vipps.no](https://portal.vipps.no).

### Vipps HTTP headers

We recommend using the following (optional) HTTP headers for all requests to the
Vipps eCom API. These headers provide useful metadata about the merchant's system,
which help Vipps improve our services, and also help in investigating problems.

These headers are **required for plugins and partners** and sent by the recent versions of
[the official Vipps plugins](https://github.com/vippsas/vipps-developers#plugins)
and we recommend all customers with direct integration with the API to also do so.

Partners must always send the `Merchant-Serial-Number` header, and we recommend
that _everyone_ sends it, also when using the merchant's own API keys.
The `Merchant-Serial-Number` header can be used with all API keys, and can
speed up any trouble-shooting of API problems quite a bit.

**Please note:** Please use self-explanatory, human readable and reasonably short
values that uniquely identify the system (and plugin).

For example, if the vendor's name is "Acme AS" and the vendor offers two different systems,
one for point of sale (POS) integrations and one for web shops,
the headers should be:

| Header | Description | Example value for POS | Example value for webshop | Example value for Vending machines |
| ------ | ----------- | --------------------- | ------------------------- | ---------------------------------- |
| `Merchant-Serial-Number`   | The MSN for the sale unit | `123456` | `123456` | `123456` |
| `Vipps-System-Name`        | The name of the ecommerce solution | `acme` | `acme` | `acme` |
| `Vipps-System-Version`     | The version number of the ecommerce solution | `1.7` | `2.6` | `2.6` |
| `Vipps-System-Plugin-Name` | The name of the ecommerce plugin | `acme-pos` | `acme-webshop` | `acme-vending` |
| `Vipps-System-Plugin-Version` | The version number of the ecommerce plugin   | `3.2` | `4.3` | `4.3` |

### Authentication

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

### QR formats

The QR code image will be returned as a URL in the response for both one-time payment QRs and merchant redirect QRs. Opening this URL will return the image in the format and resolution set in the accept header. The URL to the image will look like this:

```json
"url":"https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...."
```

The URL to the image does not require any authentication and can be shown wherever you want. Note that one-time payment QRs eventually will time out - trying to open the URL to the otp-QR image after its expiry while will return a 404. Merchant redirect QRs can be opened forever.

Below is an example merchant redirect QR:

!["Demo "](images/demo-qr.png)

#### Accept Headers

| Header Name | Header Value | Description |
| ----------- | ------------ | ----------- |
| `Accept` | `image/*` | Returns a URL pointing to a svg+xml image |
| `Accept` | `image/svg+xml` | Returns a URL pointing to a svg+xml image |
| `Accept` | `image/png` | Returns a URL pointing to a png image with 800x800 size |
| `Accept` | `text/targetUrl` | Returns the target URL of the QR *|

The `targetUrl` that points to `https://qr.vipps.no` is a shortened URL
that will be recognized and opened in the Vipps app when scanned from native camera.

\* It is possible to get the `targetUrl` of the QR if you need to
generate the QR yourselves. This will require an approval from Vipps before it is used, so we can validate
the styling and design of the QR.

If you want to create the QR code on your own, see the
[design guidelines](https://github.com/vippsas/vipps-design-guidelines#vipps-custom-qr-code)
for more details about the QR format and design.

## One-Time Payment QR codes

The Vipps QR API lets merchants generate Vipps QR codes that can be used to pay
over the counter, without requiring the Vipps user to provide their telephone
number to the merchant. These QRs are called one-time-payment QRs, and will need to be generated for each unique payment.

![One-time payment QR Flow](images/one-time-payment-qr-flow.svg)

The QR code, when scanned from either the native camera or the Vipps app, will automatically open an ecom or recurring payment in the Vipps app, where the payment can be completed. See a detailed example of [how it works](vipps-qr-one-time-payment-qpi-howitworks.md).

Every Vipps payment needs a unique `orderId`. The purchase will time out after 5 minutes, so it's not possible to print these QRs. The QR must be scanned within 5 minutes, and the user will have 5 minutes to complete the payment once opened in the app.

### Basic flow for One-Time Payment QR

1. Initiate a Vipps eCom or recurring payment
2. Receive the payment URL as response
3. Post the payment URL to the QR API
4. Receive a URL to a QR code in desired format (png or svg)

See the [quick start guide](vipps-qr-api-quick-start.md) for examples of generating QR codes.

#### Initiate a payment with the Vipps eCom API

Before creating the QR code you must initiate a payment with the Vipps eCom API as is described in depth in the
[eCom API guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate-payment-flow-phone-and-browser).

The request to the ecom initiate endpoint
[`POST:/ecomm/v2/payments`](https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Vipps-eCom-API/operation/initiatePaymentV3UsingPOST)
 will return a response like this (the `url` is truncated, but the format is correct):

```json
{
  "orderId": "acme-shop-123-order123abc",
  "url": "https://api.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLC <snip>"
}
```

Be aware that the URL is only valid for a short period of time. See the
[eCom API guide: Timeouts](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#timeouts)
for details.


#### Creation of One-Time Payment QR

Now that you have the `url` from the Vipps eCom API you can create a QR code
using the following endpoint:

[`POST:qr​/v1/`](https://vippsas.github.io/vipps-developer-docs/api/qr#operation/generateOtpQr)

Example of a request for a QR code image using `Accept: image/png`:

Headers:

```json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <snip>
Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a
Accept: image/png
Merchant-Serial-Number: 123456
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

The response will be similar to this, where the URL in the responseBody will be a link to the image as defined in the accept header:

```json
{
  "url":"https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9....",
  "expiresIn": 247
}
```

**Please note:** The `expiresIn` value is in seconds.

## Merchant Redirect QR codes

Merchant redirect QR codes allows you to make printable QR
codes that redirect the user to your webpage. This can be used for one-offs, such as tv-commercials; as well as for permanent use cases, such as stickers, billboards, and magazine ads.

!["MerchantRedirect QR Flow"](images/merchant-redirect-qr-flow.svg)

The QR code, when scanned from the native camera or the Vipps scanner, will take the customer straight to the web page.
See a detailed example of [how it works](vipps-qr-merchant-redirect-api-howitworks.md) with examples of what the user will encounter.

Merchant redirect QRs do not time out and they don't require the Vipps app to be installed.

The QR API allows for creating, updating, getting and deleting of merchant redirect QRs.
You can later change the URL through the API without generating a new QR code.

### Basic flow for Merchant Redirect QR

1. Create a merchant redirect QR
2. Later, if needed, you can:

    a. Get the QR by id

    b. Update the URL to the QR

    c. Delete the QR

See the [quick start guide](vipps-qr-api-quick-start.md) for examples of generating merchant redirect QR codes.

#### Creation of Merchant Redirect QR

To create a merchant redirect QR, make a HTTPS POST to:
[`POST:/qr/v1/merchant-redirect`](https://vippsas.github.io/vipps-developer-docs/api/qr#operation/CreateMerchantRedirectQr)

An example body like this:

```json
{
  "id": "billboard_1",
  "redirectUrl": "https://www.example.com/myProduct"
}
```

Will return a response like this:

```json
{
  "id": "billboard_1",
  "url":"https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...."
}
```

The `id` parameter is required, and is defined by the merchant. You can later use this id to update the merchant redirect QRs



#### Updating and Deletion of QRs

Updating QRs is a very similar procedure to creating them. When a QR is updated, nothing happens to the QR itself. But, when the QR is scanned, the user will be redirected to the new URL. The change is instantaneous. To update the QR, make a HTTPS PUT to:
[`PUT:/qr/v1/merchant-redirect/{id}`](https://vippsas.github.io/vipps-developer-docs/api/qr#operation/UpdateMerchantRedirectUrl)
and put the new redirectUrl in the requestBody:

```json
{
  "id": "billboard_1",
  "redirectUrl": "https://www.example.com/completelyDifferentProductThanBefore"
}
```

And the response will be exactly the same as for generating the QR the first time - and it will still point to the same image.

```json
{
  "id": "billboard_1",
  "url":"https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...."
}
```

The
[`DELETE:/qr/v1/merchant-redirect/{id}`](https://vippsas.github.io/vipps-developer-docs/api/qr#operation/DeleteMerchantRedirectQr)
does what one might expect, it deletes the QR. Once deleted, merchants can generate a new QR with the same id but the already-printed-QR will never work again.

In addition to the `DELETE`-endpoint, it is also possible to add a `ttl`-attribute in the original `POST`-request. This attribute sets how many seconds the QR will live, before it is deleted permanently.

Tip: If you want the same QR in different formats, perform `GET` calls on the same `id` with different `accept` headers and test what works best.


## Questions

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

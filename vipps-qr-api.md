# Vipps QR API v1


The Vipps QR API lets merchant generate one-time-payments QRs and merchant-redirect QRs. One-time-payment QRs (otp-QR) allows users to pay over the counter, without requiring the Vipps user to provide their telephone number to the merchant. Merchant-redirect QRs (mr-QR) will redirect the user to the merchant website. 

Both QR products share the same authentication and overall design, but have slight difference in behaviour and how they are made.


API version: 1.1.0.

Document version 1.2.0.

# Table of contents

- [Before you begin](#before-you-begin)
  * [Authentication](#authentication)
  * [QR formats](#qr-formats)
- [One Time Payment QR Codes](#one-time-payment-qr-codes)
  * [Basic flow](#basic-flow)
  * [Initiate a payment with the Vipps eCom API](#initiate-a-payment-with-the-vipps-ecom-api)
  * [Creation of One Time Payment QR](#creation-of-one-time-payment-qr)
- [Merchant Redirect QR Codes](#merchant-redirect-qr-codes)
  * [Creation of merchant redirect QR](#creation-of-merchant-redirect-qr)
  * [Updating and Deletion of QRs](#updating-and-deletion-of-qrs)
- [Questions?](#questions-)


# Before you begin

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

## QR formats

The QR code image will be returned as a URL in the response for both otp- and mr-QRs. Opening this URL will return the image in the format (and resolution) set in the accept header. The URL to the image will look like this:
```json
"url":"https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...."
```
The URL to the images does not require any authentication, and can be shown wherever you want. Note that oneTimePayment QRs eventually will time out - trying to open the URL to the otp-QR image after its expiry while will return a 404. Merchant redirect QRs can be opened forever.

Below is an example merchant Redirect QR to showcase the design. The URL returned from the QR api for this QR can be opened 
[here](https://qr-generator-prod-app-service.azurewebsites.net/qr-generator/v1?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb3JtYXQiOiJpbWFnZS9zdmcreG1sIiwidXJsIjoiaHR0cHM6Ly9zaG9ydC52aXBwcy5uby9yLzk0V1BwQWZYIiwiaXNzIjoiUXItQXBpIiwibmJmIjoxNjQ3NTMxODY2LCJleHAiOjE2NDc1MzU0NjYsImlhdCI6MTY0NzUzMTg2Nn0.1PydxyBWFsCTU8BrPLOawap8F7D5DcQAeOGisxB_Oho).
<p align="center">
  <img src="images/demoQr.svg" alt="Demo QR" width="250">
</p>

### Accept Headers:
| Header Name | Header Value | Description |
| ----------- | ------------ | ----------- |
| `Accept` | `image/*` | Returns a URL pointing to a svg+xml image |
| `Accept` | `image/svg+xml` | Returns a URL pointing to a svg+xml image |
| `Accept` | `image/png` | Returns a URL pointing to a png image with 800x800 size |
| `Accept` | `text/targetUrl` | Returns the target URL of the QR *|

\* It is possible to get the `targetUrl` of the QR if you need to
generate the QR yourselves. This will require an approval from Vipps before it is used, so we can validate
the styling and design of the QR.

The `targetUrl` that points to `https://qr.vipps.no` is a shortened URL
that will be recognized and opened in the Vipps app when scanned from native camera.

If you want to make the QR code on your own: See the
[design guidelines](https://github.com/vippsas/vipps-design-guidelines#vipps-custom-qr-code)
for more details about the QR format and design.

# One Time Payment QR codes
<p align="center">
  <img src="images/OneTimePaymentQr.svg" alt="OneTimePayment QR Flow">
</p>

* [API Documentation with examples here](https://vippsas.github.io/vipps-qr-api/redoc.html#tag/One-time-payment-QR)

The following section will explain how to generate one-time-payment QR codes. Every Vipps payment needs a unique `orderId`, so it's not possible to print these, as they will only be valid for 5 minutes. The QR must be scanned within 5 minutes, and the user will have 5 minutes to complete the payment once opened in the app.

The QR code, when scanned, will take the customer straight to the payment
screen in the Vipps app. They are scannable from both native camera and the scanner in the Vipps app.
## Basic flow
1. Initiate a Vipps eCom or recurring payment
    - `POST:/ecomm/v2/payments` 
2. Receive the payment URL as response
3. Post the payment URL to the QR API
    - `POST:/qr/v1` 
4. Receive a URL to a QR code in desired format (png or svg)

## Initiate a payment with the Vipps eCom API

Before creating the QR code you must initiate a payment with the Vipps eCom API as is described in depth in the
[eCom API guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate-payment-flow-phone-and-browser).

The request to the ecom initiate endpoint
[`POST:/ecomm/v2/payments`](https://vippsas.github.io/vipps-ecom-api/#/Vipps%20eCom%20API/initiatePaymentV3UsingPOST)
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

## Creation of One Time Payment QR


Now that you have the `url` from the Vipps eCom API you can create a QR code
using the following endpoint:

[`POST:qr​/v1/`](https://vippsas.github.io/vipps-qr-api/redoc.html#operation/generateOtpQr)

Example of a request for a QR code image using `Accept: image/png`:

Headers:

```
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

# Merchant Redirect QR codes
<p align="center">
  <img src="images/MerchantRedirectQr.svg" alt="MerchantRedirect QR Flow">
</p>

* [API Documentation with examples here](https://vippsas.github.io/vipps-qr-api/redoc.html#tag/Merchant-redirect-QR)

The following section will explain how to generate merchant redirect QR codes. The QR api allows for creating, updating, getting and deleting of merchant redirect QRs. These are pretty simple in function - all they do is redirect the user to the webpage provided by the merchant.

The QR code, when scanned from native camera or the Vipps scanner, will take the customer straight to webpage. These QRs don't require the Vipps app to be installed.

## Creation of merchant redirect QR
To create a merchantRedirect QR, make a HTTPS POST to:
[`POST:/qr/v1/merchant-redirect`](https://vippsas.github.io/vipps-qr-api/redoc.html#operation/CreateMerchantRedirectQr)

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

## Updating and Deletion of QRs
Updating QRs is a very similar procedure to creating them. When a QR is updated, nothing happens to the QR itself. But, when the QR is scanned, the user will be redirected to the new URL. The change is instantaneous. To update the QR, make a HTTPS PUT to:
[`PUT:/qr/v1/merchant-redirect/{id}`](https://vippsas.github.io/vipps-qr-api/redoc.html#operation/UpdateMerchantRedirectUrl)
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
[`DELETE:/qr/v1/merchant-redirect/{id}`](https://vippsas.github.io/vipps-qr-api/redoc.html#operation/DeleteMerchantRedirectQr)
does what one might expect, it deletes the QR. Once deleted, merchants can generate a new QR with the same id but the already-printed-QR will never work again. 

Tip: If you want the same QR in different formats, do GET calls on the same `id` with different accept headers and test what works best.
# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

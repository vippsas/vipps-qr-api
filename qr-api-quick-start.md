<!-- START_METADATA
---
title: Quick start for the QR API
sidebar_label: Quick start
sidebar_position: 20
description: Quick steps for getting started with the QR API.
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
END_METADATA -->

# Quick start

## Before you begin

The provided example values in this guide must be changed with the values for your sales unit and user.
This applies for API keys, HTTP headers, reference, phone number, etc.

## Your first QR code

### Step 1 - Setup

You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal.

You will need the following values, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started):

* `client_id` - Client_id for a test sales unit.
* `client_secret` - Client_id for a test sales unit.
* `Ocp-Apim-Subscription-Key` - Subscription key for a test sales unit.
* `merchantSerialNumber` - The unique ID for a test sales unit.
* `MobileNumber` - The phone number for the test app profile you have received or registered. This is your test mobile number *without* country code.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

In Postman, import the following files:

* [QR API Postman collection](/tools/qr-api-postman-collection.json)
* [API Global Postman environment](https://developer.vippsmobilepay.com/internal-tools/global-postman-environment.json)

🔥 **To reduce risk of exposure, never store production keys in Postman or any similar tools.** 🔥

Update the *Current Value* field in your Postman environment with your **Merchant Test** keys.
Use *Current Value* field for added security, as these values are not synced to the cloud.

</TabItem>
<TabItem value="curl">

No additional setup needed :)

</TabItem>
</Tabs>

### Step 2 - Authentication

Get an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`](https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost).

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get Access Token
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
--data ''
```

</TabItem>
</Tabs>

The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

## Merchant Redirect QR

A merchant redirect QR contains a link to your webshop. When the user scans this with their phone, your website will open.

![QR code](images/demo-qr.png)

Generate a merchant redirect QR with:
[`POST:/qr/v1/merchant-redirect`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/CreateMerchantRedirectQr).

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Merchant Redirect QR > Generate QR
```

The `qr-id` variable is now set in the environment for use with subsequent calls.

*Ctrl+click* the link to see the QR code. Scanning the QR should open the specified URL on your phone.

The result from this request provides a URL with its own JWT token, which will expire.
Get a new token by calling `Get QR by id`.

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/qr/v1/merchant-redirect \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X POST \
-d '{
    "redirectUrl": "https://example.com/mywebshop",
    "id": "UNIQUE-QR-ID"
}'
```

Enter the returned link into a browser. It will show the QR code. Scanning the QR should open the specified URL on your phone.

The result from this request provides a URL with its own JWT token, which will expire.
Get a new token by calling
[`GET:/qr/v1/merchant-redirect/{qr-id}`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/GetMerchantRedirectQrById).

</TabItem>
</Tabs>

Relevant examples:

* [Static QR directing to the merchant site for payment](https://developer.vippsmobilepay.com/docs/solutions/vending-machines/qr-to-merchant-site-payment-only/)
* [Static QR directing to the merchant site for product selection](https://developer.vippsmobilepay.com/docs/solutions/vending-machines/qr-to-merchant-site-product-selection/)

## One-time payment QR

A one-time payment QR (also called dynamic QR) is connected to a payment. When the user scans this QR with their phone,
the Vipps app will open and present them with the payment request.

If you are using the ePayment API in your solution, you do not need to use the QR API.
See
[Dynamic QR directing to the app for payment](https://developer.vippsmobilepay.com/docs/solutions/vending-machines/one-time-payment/) for an example.

<details>
<summary>Legacy method for eCom API</summary>
<div>

Create a payment and get the unique payment reference.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Initiate Payment
```

The `orderId` and `vippsLandingPageUrl` variables are now in the environment of this Postman example.

</TabItem>
<TabItem value="curl">

```bash
curl --location 'https://apitest.vipps.no/ecomm/v2/payments/' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
  "customerInfo": {
    "mobileNumber": "91234567"
  },
  "merchantInfo": {
    "merchantSerialNumber": "123456",
    "callbackPrefix":"https://example.com/vipps/callbacks-for-payment-update-from-vipps",
    "fallBack": "https://example.com/vipps/fallback-result-page-for-both-success-and-failure/acme-shop-123-order123abc",
  },
  "transaction": {
    "amount": 49900,
    "orderId": "UNIQUE-PAYMENT-REFERENCE",
    "transactionText": "One pair of socks.",
}
}'
```

Note that `orderId` must be unique for each payment you create.

</TabItem>
</Tabs>

Take note of the URL that is returned in the response body and provide it in the
[POST:/qr/v1](https://developer.vippsmobilepay.com/api/qr/#tag/One-time-payment-QR/operation/generateOtpQr)
request to generate the one-time payment QR.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Generate OTP QR
```

This supplies `vippsLandingPageUrl` to
[`POST:/qr/v1`](https://developer.vippsmobilepay.com/api/qr#tag/One-time-payment-QR/operation/generateOtpQr)
to provide a URL that can be used to show a QR code.

*Ctrl+click* the link to see the QR code. Scanning the QR should open the test app on your phone and allow you to complete the one-time purchase.

</TabItem>
<TabItem value="curl">

```bash
curl --location 'https://apitest.vipps.no/qr/v1' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
    "url": "https://apitest.vipps.no/dwo-api-application/v1/deeplink/vippsgateway?v=2&token=eyJraWQiOiJqd3RrZXkiLCJhbGciOiJSUzI1NiJ<truncated>"
}'
```

</TabItem>
</Tabs>

</div>
</details>

## Next steps

See the [QR API guide](qr-api-guide.md) to read about the concepts and details.

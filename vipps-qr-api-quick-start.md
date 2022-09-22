<!-- START_METADATA
---
title: Postman
sidebar_position: 40
---
END_METADATA -->

# Postman

[Postman](https://www.getpostman.com/) is a common tool for working with REST APIs.
We offer a [Postman Collection](https://www.getpostman.com/collection) to make development easier.
See the [Postman documentation](https://www.getpostman.com/docs/) for more information about using Postman.

By following the steps below, you can make calls to all the
endpoints, and see the full `request` and `response` for each call.

We also have a short [getting started guide to Postman](https://github.com/vippsas/vipps-developers/blob/master/vipps-quick-start-guides.md).

## Setting up Postman

### Step 1: Get the Postman Collection

Import the collection by following the steps below:

1. Click `Import` in the upper-left corner.
2. Import the [vipps-qr-api-quick-start-collection.json](./tools/vipps-qr-api-quick-start-collection.json) file.

### Step 2: Import the Postman Environment

1. Click `Import` in the upper-left corner.
2. Import the [vipps-qr-api-quick-start-environment.json](./tools/vipps-qr-api-quick-start-environment.json) file.

### Step 3: Set up your Postman Environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   - `client-id`
   - `client-secret`
   - `merchantSerialNumber`
   - `Ocp-Apim-Subscription-Key`

### Step 4: Run tests

#### 1. Get access token

Send request `Get Access Token`. This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.

#### 2. Run Merchant Redirect QR Examples

Under the *Merchant Redirect QR* folder:

1. Send request `Generate QR`.

   This creates a QR that works as a redirect back to the merchant. The website is specified as the `redirectUrl` in the [`POST:/qr/v1/merchant-redirect`](https://vippsas.github.io/vipps-developer-docs/api/qr#tag/Merchant-redirect-QR/operation/CreateMerchantRedirectQr) request.

   The `qr-id` variable is now set in the environment for use with subsequent calls.

   Ctrl+click the link to see the QR code. Scanning the QR should open the website on your phone.

   **Please note:** The result from Generate QR provides a url with its own JWT token. This token will expire. If so, get a new token by calling `Get QR`.

2. Send request `Get QR by id`.

   This gets the QR for the specified `qr-id` in
[`GET:/qr/v1/merchant-redirect/{{qr-id}}`](https://vippsas.github.io/vipps-developer-docs/api/qr#tag/Merchant-redirect-QR/operation/GetMerchantRedirectQrById).

   Ctrl+click the link to see the QR code. When you scan it, it will take you to the specified URL.

3. Send request `Update redirectUrl by id`.

   This changes the URL for the QR code with the specified `qr-id` in
[`PUT:/qr/v1/merchant-redirect/{{qr-id}}`](https://vippsas.github.io/vipps-developer-docs/api/qr#tag/Merchant-redirect-QR/operation/UpdateMerchantRedirectUrl).

4. Send request `Delete QR by id`.

   This deletes the QR code with the specified `qr-id` in
[`DEL:/qr/v1/merchant-redirect/{{qr-id}}`](https://vippsas.github.io/vipps-developer-docs/api/qr#tag/Merchant-redirect-QR/operation/DeleteMerchantRedirectQr).

5. Send request `Get all QRs`.

   This gets all the QRs by calling [`GET:/qr/v1/merchant-redirect`](https://vippsas.github.io/vipps-developer-docs/api/qr#tag/Merchant-redirect-QR/operation/GetAllMerchantRedirectQrs) request.

See the [QR API Specifications](https://vippsas.github.io/vipps-developer-docs/api/qr) for details about the calls.

#### 3. Run One-Time Payment QR Examples

Under the *One-Time Payment QR* folder:

1. Send request `Initiate Payment`.

   This uses [`POST:/v3/psppayments/init/`](https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Vipps-eCom-API/operation/initiatePaymentV3UsingPOST)
   from the [Vipps eComm API](https://github.com/vippsas/vipps-ecom-api).

   The `orderId` and `vippsLandingPageUrl` variables are now in the environment of this Postman example

1. Send request `Generate OTP QR`. This supplies `vippsLandingPageUrl` to
 [`POST:/qr/v1`](https://vippsas.github.io/vipps-developer-docs/api/qr/#tag/One-time-payment-QR/operation/generateOtpQr) to provide a url that can be used to show a QR code.

   Ctrl+click the link to see the QR code. Scanning the QR should open the test app on your phone and allow you to complete the one-time purchase.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-qr-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

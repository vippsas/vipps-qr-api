<!-- START_METADATA
---
title: Quick start
sidebar_position: 20
---
END_METADATA -->

# Quick start

Use the QR API to generate QR codes that redirect the user back to a URL.
You can get a list of all QR codes or delete a QR.
If needed, you can update the redirect URL at a later time.

<!-- START_TOC -->

## Table of Contents

- [Quick start](#quick-start)
  - [Table of Contents](#table-of-contents)
  - [Postman](#postman)
    - [Prerequisites](#prerequisites)
    - [Step 1: Get the Vipps Postman collection and environment](#step-1-get-the-vipps-postman-collection-and-environment)
    - [Step 2: Import the Vipps Postman files](#step-2-import-the-vipps-postman-files)
    - [Step 3: Set up Postman environment](#step-3-set-up-postman-environment)
  - [Make API calls](#make-api-calls)
    - [A Merchant redirect QR](#a-merchant-redirect-qr)
    - [One-Time Payments](#one-time-payments)
  - [Questions?](#questions)

<!-- END_TOC -->

Document version 1.0.2.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://github.com/vippsas/vipps-developers/blob/master/vipps-quick-start-guides.md) for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps QR API Postman collection](tools/vipps-qr-api-postman-collection.json)
* [Vipps QR API Postman environment](tools/vipps-qr-api-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant id.
   * `mobileNumber` - The mobile number for the test app profile you have received or registered.

## Make API calls

For all of the following, you will start by sending request `Get Access Token`.
This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.
See the
[API reference](https://vippsas.github.io/vipps-developer-docs/api/qr)
for details about the calls.

### A Merchant redirect QR

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

### One-Time Payments

Under the *One-Time Payment QR* folder:

1. Send request `Initiate Payment`.

   This uses [`POST:/ecomm/v2/payments`](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api.md#initiate) or [`POST:/v3/psppayments/init/`](https://github.com/vippsas/vipps-psp-api/blob/master/vipps-psp-api.md#initiate-payment)
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

<!-- START_METADATA
---
title: QR API extended Postman guide
sidebar_label: Quick start
sidebar_position: 20
description: Quick start guide for the using the QR API with Postman.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Quick start

<!-- START_COMMENT -->
ℹ️ Please use the new documentation:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/qr-api).
<!-- END_COMMENT -->

Use the QR API to generate QR codes that redirect the user back to a URL.
You can get a list of all QR codes or delete a QR.
If needed, you can update the redirect URL at a later time.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://developer.vippsmobilepay.com/docs/quick-start-guides)
for information about getting your test environment set up.

### Step 1: Get the Postman collection and environment

Save the following files to your computer:

* [QR API Postman collection](/tools/vipps-qr-api-postman-collection.json)
* [Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Postman files

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
   * `merchantSerialNumber` - Merchant ID.
   * `mobileNumber` - The mobile number for the test app profile you have received or registered.

## Make API calls

For all the following, you will start by sending request `Get Access Token`.
This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.
See the
[API reference](https://developer.vippsmobilepay.com/api/qr)
for details about the calls.

### A Merchant redirect QR

Under the *Merchant Redirect QR* folder:

1. Send request `Generate QR`.

   This creates a QR that works as a redirect back to the merchant. The website is specified as the `redirectUrl` in the
   [`POST:/qr/v1/merchant-redirect`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/CreateMerchantRedirectQr)
   request.

   The `qr-id` variable is now set in the environment for use with subsequent calls.

   Ctrl+click the link to see the QR code. Scanning the QR should open the website on your phone.

   **Please note:** The result from `Generate QR` provides a URL with its own JWT token. This token will expire. If so, get a new token by calling `Get QR`.

2. Send request `Get QR by id`.

   This gets the QR for the specified `qr-id` in
[`GET:/qr/v1/merchant-redirect/{{qr-id}}`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/GetMerchantRedirectQrById).

   Ctrl+click the link to see the QR code. When you scan it, it will take you to the specified URL.

3. Send request `Update redirectUrl by id`.

   This changes the URL for the QR code with the specified `qr-id` in
   [`PUT:/qr/v1/merchant-redirect/{{qr-id}}`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/UpdateMerchantRedirectUrl).

4. Send request `Delete QR by id`.

   This deletes the QR code with the specified `qr-id` in
   [`DEL:/qr/v1/merchant-redirect/{{qr-id}}`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/DeleteMerchantRedirectQr).

5. Send request `Get all QRs`.

   This gets all the QRs by calling
   [`GET:/qr/v1/merchant-redirect`](https://developer.vippsmobilepay.com/api/qr#tag/Merchant-redirect-QR/operation/GetAllMerchantRedirectQrs) request.

See the [QR API Specifications](https://developer.vippsmobilepay.com/api/qr) for details about the calls.

### One-Time Payments

Under the *One-Time Payment QR* folder:

1. Send request `Initiate Payment`.

   This uses [`POST:/ecomm/v2/payments`](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api#initiate) or
   [`POST:/v3/psppayments/init/`](https://developer.vippsmobilepay.com/docs/APIs/psp-api/vipps-psp-api#initiate-payment)
   from the [eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api).

   The `orderId` and `vippsLandingPageUrl` variables are now in the environment of this Postman example

1. Send request `Generate OTP QR`. This supplies `vippsLandingPageUrl` to
   [`POST:/qr/v1`](https://developer.vippsmobilepay.com/api/qr#tag/One-time-payment-QR/operation/generateOtpQr)
   to provide a URL that can be used to show a QR code.

   Ctrl+click the link to see the QR code. Scanning the QR should open the test app on your phone and allow you to complete the one-time purchase.

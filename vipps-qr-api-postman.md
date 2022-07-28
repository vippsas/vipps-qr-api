# Postman

[Postman](https://www.getpostman.com/) is a common tool for working with REST APIs.
We offer a [Postman Collection](https://www.getpostman.com/collection) to make development easier.
See the [Postman documentation](https://www.getpostman.com/docs/) for more information about using Postman.

By following the steps below, you can make calls to all the
endpoints, and see the full `request` and `response` for each call.

We also have a short [getting started guide to Postman](https://github.com/vippsas/vipps-developers/blob/master/postman-guide.md).

## Setting up Postman

### Step 1: Get the Postman Collection

Import the collection by following the steps below:

1. Click `Import` in the upper-left corner.
2. Import the [vipps-qr-api-postman-collection.json](https://raw.githubusercontent.com/vippsas/vipps-qr-api/main/tools/vipps-qr-api-postman-collection.json) file.

### Step 2: Import the Postman Environment

1. Click `Import` in the upper-left corner.
2. Import the [vipps-qr-api-postman-environment.json](https://raw.githubusercontent.com/vippsas/vipps-qr-api/main/tools/vipps-qr-api-postman-environment.json) file.

### Step 3: Set up your Postman Environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   - `client-id`
   - `client-secret`
   - `merchantSerialNumber`
   - `Ocp-Apim-Subscription-Key`


### Step 4: Run Merchant Redirect QR Tests

1. Send request `GetAccessToken`. This provides you with access to the API.

2. Send request `Generate QR`. This creates a QR that works as a redirect back to the merchant. The website is specified as the `redirectUrl` in the [`POST:/qr/v1/merchant-redirect`](https://vippsas.github.io/vipps-qr-api/redoc.html#tag/Merchant-redirect-QR/operation/CreateMerchantRedirectQr) request.

   Ctrl+click the link to see the QR code. Scanning the QR should open the website on your phone.

   **Please note:** The result from Generate QR provides a url with its own JWT token. This token will expire. If so, get a new token by calling `Get QR`.

3. Send request `Get QR`. This gets the QR for the specified merchant-id by using
[https://{{url}}/qr/v1/merchant-redirect/{{merchant-id}}](https://vippsas.github.io/vipps-qr-api/redoc.html#tag/Merchant-redirect-QR/operation/GetMerchantRedirectQrById) request.

   In this example, the `merchant-id` is set to the `merchantSerialNumber` in step 2.

   Ctrl+click the link to see the QR code.

See the [QR API Specifications](https://vippsas.github.io/vipps-qr-api/redoc.html) for details about the calls.


### Step 5: Run One Time Payment QR Tests

To be provided.

For now, you can test out how to generate QR codes for one-time payments from the eCom API postman collection.
See the [eCom postman guide](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-postman.md) for details.


# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-qr-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

# Vipps Vipps QR API v1

## One-Time Payment QR 
This repository contains developer resources for the Vipps QR API.

The Vipps QR API lets merchants generate Vipps QR codes that can be used to pay
over the counter, without requiring the Vipps user to provide their telephone
number to the merchant.

The QR code, when scanned and opened, will redirect the user to the Vipps
landing page, which on the phone will automatically trigger a switch to the
Vipps app where they can pay the merchant.

For more information:
* [API Guide](vipps-qr-api.md)
* [Vipps Developers](https://github.com/vippsas/vipps-developers): The starting point for Vipps developers.
* [Getting Started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md): Information about API keys, product activation.

You can peruse the API reference documentation as:
* [Swagger UI](https://vippsas.github.io/vipps-qr-api/)
* [ReDoc](https://vippsas.github.io/vipps-qr-api/redoc.html)

## Merchant Redirect QR
💥 DRAFT: This is unfinished work and subject to change. 💥  

Merchant Redirect QRs will be a product where merchants can make QRs that simply point to their own websites.
This solution is meant for more permanent usecases such as stickers at the counter, billboards, TV-commercials etc. where you want the customer to be able to be redirected at a product page directly from scanning the QR.

![uml diagram](images/uml-of-merchant-flow.png)

This endpoint will make it possible to generate a vipps-branded QR with the ability to change the url it points to. That is possible because the QR points to a url on the format `qr.vipps.no/r/abc123` and what that url redirects to simply has to be updated in Vipps´ database. These QRs will be scannable from both vipps QR scanner and native camera scanner, and will always redirect the user to the targetUrl that you decided.

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

# Vipps Vipps QR API v1

There are two ways to use the Vipps QR API:
* [One-Time Payment QR](#one-time-payment-qr)
* [Merchant Redirect QR](merchant-redirect-qr)

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

Merchant redirect QR codes will be a product where merchants can make static QR
codes that simply contain the URL to the merchant's website.

This solution is meant for more permanent use cases such as stickers at the counter,
billboards, TV-commercials, etc. where you want the customer to be able to scan a
Vipps-branded QR code and be sent directly to a product page.

![uml diagram](images/uml-of-merchant-flow.png)

This API endpoint will make it possible to generate a Vipps-branded QR code with the
ability to change the URL it points to. That is possible because the QR code points
to a URL on the format `qr.vipps.no/r/abc123` and what that URL redirects to
simply has to be updated in Vipps´ database. These QR codes will be scannable from
both the Vipps QR scanner and the native camera scanner, and will always redirect the
user to the `targetUrl` that you decided.

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

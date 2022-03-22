# Vipps QR API v1 FAQ

API version: 1.1.0.

Document version 1.2.0.

## Table of Contents

* [See the Vipps eCom API FAQ](#see-the-vipps-ecom-api-faq)
* [Can we make our own QR code based on the Vipps deeplink URL?](#can-we-make-our-own-qr-code-based-on-the-vipps-deeplink-url)
* [Why are there extra API calls to retrieve the QR code?](#why-are-there-extra-api-calls-to-retrieve-the-qr-code)
* [Questions?](#questions)

## See the Vipps eCom API FAQ

It contains answers to all(?) common questions about Vipps payments:
[Vipps eCom API FAQ](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api-faq.md).

## Cant we make our own QRs for payment and redirects?

Technically, yes. Is it a good idea, and will it work? No.

Firstly, we have a security aspect. We only allow QRs on the qr.vipps.no domain to be scanned within the Vipps app, to ensure that we have control of everything that is scanned. For payment QRs, using our own domain also means we have full control over universal linking. These two aspect forces us to only allow our own QR codes for payments.

Addition: Vipps is an extremely strong brand in Norway, with a very high level of
trust. The Vipps-branded QR codes will look familiar to users, and in our
experience result in a higher completion rate. The Vipps QR codes also contain very little data, have good contrast and are very reliable to scan.

## Why are there extra API calls to retrieve the QR code?

The Vipps QR API is an addition to the
[Vipps eCom API](https://github.com/vippsas/vipps-ecom-api).
Merchants can integrate with the Vipps eCom API for many different use cases.
The Vipps QR API adds "just" the QR code functionality.

Yes, there is one extra API calls, but the overhead is minimal.
With current HTTP technology, the user will not notice any delay.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-qr-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

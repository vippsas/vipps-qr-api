# Vipps Vipps QR API v1 FAQ

💥 DRAFT: This is unfinished work and subject to change. 💥

API version: 1.0.0.

Document version 0.0.1.

# Table of Contents

* [See the Vipps eCom API FAQ](#see-the-vipps-ecom-api-faq)
* [Can we make our own AR code based on the4 Vipps deeplink URL?](#can-we-make-our-own-ar-code-based-on-the4-vipps-deeplink-url)
* [Why are there extra API calls to retrieve the QR code?](#why-are-there-extra-api-calls-to-retrieve-the-qr-code)
* [Questions?](#questions)

# See the Vipps eCom API FAQ

It contains answers to all(?) common questions about Vipps payments:
[Vipps eCom API FAQ](https://github.com/vippsas/vipps-ecom-api/blob/master/vipps-ecom-api-faq.md).

# Can we make our own AR code based on the4 Vipps deeplink URL?

Technically: Yes.

But: Vipps is an extremely strong brand in Norway, with a very high level of
trust. The Vipps-branded QR codes will look familiar to users, and in our
experience result in a higher completion rate.

The Vipps QR codes contain shorter URLs (less data) and are easier
and more reliable to scan.

Also: We do _require_ merchants to use the Vipps QR API, and not make their
own QR code hacks as part of the Vipps payment process.

# Why are there extra API calls to retrieve the QR code?

The Vipps QR API is an addition to the
[Vipps eCom API](https://github.com/vippsas/vipps-ecom-api).
Merchants can integrate with the Vipps eCom API for many different use cases.
The Vipps QR API adds "just" the QR functionality.

Yes, there will be some extra API calls, but the overhead is minimal.
With current HTTP technology, the user will not notice any delay.

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-qr-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
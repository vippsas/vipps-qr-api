<!-- START_METADATA
---
title: FAQ
sidebar_position: 45
---
END_METADATA -->

# Vipps QR API v1 FAQ

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

API version: 1.1.1.

Document version 1.2.3.

<!-- START_TOC -->

## Table of Contents

* [See the Vipps FAQs](#see-the-vipps-faqs)
* [Can we make our own QRs for payment and redirects?](#can-we-make-our-own-qrs-for-payment-and-redirects)
* [Why are there extra API calls to retrieve the QR code?](#why-are-there-extra-api-calls-to-retrieve-the-qr-code)
* [Questions?](#questions)

<!-- END_TOC -->

## See the Vipps FAQs

It contains answers to all(?) common questions about Vipps payments:
[Vipps FAQ](https://github.com/vippsas/vipps-developers/tree/master/faqs).

## Can we make our own QRs for payment and redirects?

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

Yes, there is one extra API call, but the overhead is minimal.
With current HTTP technology, the user will not notice any delay.

## What happens with the user after the payment?

In the One-Time Payment QR flow, the user will be sent to the receipt view in the Vipps app once the payment is complete. The Merchant Redirect flow will redirect the user to the URL that you specify.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-qr-api/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).

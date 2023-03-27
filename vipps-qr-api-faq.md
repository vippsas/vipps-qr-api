<!-- START_METADATA
---
title: FAQ
sidebar_position: 45
pagination_next: null
---
END_METADATA -->

# Vipps QR API FAQ

Here are the QR API FAQs. See the
[Vipps QR API guide](vipps-qr-api.md)
for all the details.

For more common Vipps questions, see:

* [Vipps API General FAQ](https://developer.vippsmobilepay.com/docs/vipps-developers/faqs)

API version: 1.1.1.

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/qr-api).

## Table of Contents

* [See the Vipps FAQs](#see-the-vipps-faqs)
* [Can we make our own QRs for payment and redirects?](#can-we-make-our-own-qrs-for-payment-and-redirects)
* [Why are there extra API calls to retrieve the QR code?](#why-are-there-extra-api-calls-to-retrieve-the-qr-code)

<!-- END_COMMENT -->

## See the Vipps FAQs

It contains answers to all(?) common questions about Vipps payments:
[Vipps FAQ](https://developer.vippsmobilepay.com/docs/vipps-developers/faqs).

## Can we make our own QRs for payment and redirects?

Technically, yes. Is it a good idea, and will it work? No.

Firstly, we have a security aspect. We only allow QRs on the qr.vipps.no domain to be scanned within the Vipps app, to ensure that we have control of everything that is scanned. For payment QRs, using our own domain also means we have full control over universal linking. These two aspect forces us to only allow our own QR codes for payments.

Addition: Vipps is an extremely strong brand in Norway, with a very high level of
trust. The Vipps-branded QR codes will look familiar to users, and in our
experience result in a higher completion rate. The Vipps QR codes also contain very little data, have good contrast and are very reliable to scan.

## Why are there extra API calls to retrieve the QR code?

The Vipps QR API is an addition to the
[Vipps eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api).
Merchants can integrate with the Vipps eCom API for many different use cases.
The Vipps QR API adds "just" the QR code functionality.

Yes, there is one extra API call, but the overhead is minimal.
With current HTTP technology, the user will not notice any delay.

## What happens with the user after the payment?

In the One-Time Payment QR flow, the user will be sent to the receipt view in the Vipps app once the payment is complete. The Merchant Redirect flow will redirect the user to the URL that you specify.

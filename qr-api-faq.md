<!-- START_METADATA
---
title: QR API Frequently Asked Questions
sidebar_label: FAQ
sidebar_position: 45
description: Frequently asked questions for the QR API.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# QR API FAQ

<!-- START_COMMENT -->
ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/qr-api).
<!-- END_COMMENT -->

Here are the QR API Frequently Asked Questions (FAQ).
See the [QR API guide](qr-api-guide.md) for all the technical details.
For general information and questions, please check in the
[Knowledge base](https://developer.vippsmobilepay.com/docs/knowledge-base/).

API version: 1.1.1.


## Can we make our own QRs for payment and redirects?

Technically, yes. Is it a good idea, and will it work? No.

Firstly, we have a security aspect. We only allow QRs on the qr.vipps.no domain to be scanned within the Vipps app, to ensure that we have control of everything that is scanned. For payment QRs, using our own domain also means we have full control over universal linking. These two aspect forces us to only allow our own QR codes for payments.

Addition: Vipps is an extremely strong brand in Norway, with a very high level of
trust. The Vipps-branded QR codes will look familiar to users, and in our
experience result in a higher completion rate. The Vipps QR codes also contain very little data, have good contrast and are very reliable to scan.

## Why are there extra API calls to retrieve the QR code?

The QR API is an addition to the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api).
Merchants can integrate with the eCom API for many use cases.
The QR API adds "just" the QR code functionality.

Yes, there is one extra API call, but the overhead is minimal.
With current HTTP technology, the user will not notice any delay.

## What happens with the user after the payment?

In the One-Time Payment QR flow, the user will be sent to the receipt view in the Vipps app once the payment is complete. The Merchant Redirect flow will redirect the user to the URL that you specify.

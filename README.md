# Vipps QR API v1
This repository contains developer resources for the Vipps QR API. The Vipps QR API can be used to create the following types of QR:
* [One Time Payment QR](#one-time-payment-qr)
* [Merchant Redirect QR](#merchant-redirect-qr)

For more information:
* [API Guide](vipps-qr-api.md): The Vipps QR API guide
* [API FAQ](vipps-qr-api-faq.md): The Vipps QR API FAQ
* [Vipps Developers](https://github.com/vippsas/vipps-developers): The starting point for Vipps developers.
* [Getting Started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md): Information about API keys, product activation.

You can peruse the API reference documentation as:
* [Swagger UI](https://vippsas.github.io/vipps-qr-api/)
* [ReDoc](https://vippsas.github.io/vipps-qr-api/redoc.html)

# One Time Payment QR
The Vipps QR API lets merchants generate Vipps QR codes that can be used to pay
over the counter, without requiring the Vipps user to provide their telephone
number to the merchant. These QRs are called one-time-payment QRs, and will need to be generated for each unique payment.

!["OneTimePayment QR Flow](images/one-time-payment-qr-flow.svg)


The QR code, when scanned from either native camera or the Vipps app, will automatically open a ecom or recurring payment in the Vipps app where the payment can be completed.

* [One Time Payment QR Code Api Guide](vipps-qr-api.md#one-time-payment-qr-codes)
* [Detailed how it works Guide](how-it-works/one-time-payment-qr.md)

# Merchant Redirect QR

Merchant redirect QR codes is a product where merchants can make printable QR
codes that redirects the user back to the merchants webpage. This solution can be used for one-offs as tv-commercials, and permanent use cases such as stickers, billboards, magazine ads, etc.

!["MerchantRedirect QR Flow"](images/merchant-redirect-qr-flow.svg)

The QR codes will always be scannable from both the Vipps QR scanner and the native
camera app, and will redirect the user to the `redirectUrl` configured by the merchant. The redirectUrl is changeable through the api, so the URL the user is redirected to can be changed by the merchant after they are printed out.

* [Merchant Redirect QR Code Api Guide](vipps-qr-api.md#merchant-redirect-qr-codes)
* [Detailed how it works Guide](how-it-works/merchant-redirect-qr.md)


# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).

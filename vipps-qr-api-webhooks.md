<!-- START_METADATA
---
title: QR API Webhooks
sidebar_label: Webhooks
sidebar_position: 35
description: Find information on the use of webhooks in the QR solution
pagination_prev: Null
pagination_next: Null
---
END_METADATA -->

# QR Api Webhooks
The QR solution currently has one webhook a merchant can subscribe to.

The central part of the Merchant Callback QR flow is the callback, that will be sent to the merchant when a customer scans the QR.
To receive these callbacks, the merchant needs to register a subscription to the *user.checkin.v1* event. 

Here is the payload for the *user.checkin.v1* event:

| Name | Type | Description |
| ---- | ---- | ----------- |
| customerToken | base64 string | A reference to the customer. Should be used when initiating a payment through the ePayments API |
| merchantQrId | string | The id of the QR code that has been scanned. Defined by the merchant when the QR was created |
| merchantSerialNumber | string | A unique id of the sales unit that the QR scanned belongs to |
| initiatedAt | Timestamp in ISO 8601 format | The timestamp of when the customer scanned the QR |


Example of *user.checkin.v1* payload:
````
{
    "customerToken": "wbA8ceVRKkoYiQAVELHeFCC3Sn5dtNCvvEtVPiOT77j6wx7uR965AG6Q+q0ATP4=",
    "merchantQrId": "d8b7d76d-49aa-48b8-90c6-38779372c163",
    "merchantSerialNumber": "12345",
    "initiatedAt": "2023-10-06T10:45:40.3061965Z"
}
````

See the [Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/) for more information.
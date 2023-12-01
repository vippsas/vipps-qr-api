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

# QR API Webhooks
The QR solution currently has one webhook event a merchant can subscribe to.

The central part of the Merchant Callback QR flow is the callback that will be sent to the merchant when a customer scans the QR.
To receive these callbacks, the merchant needs to register a subscription to the `user.checked-in.v1` webhook event. 

Here is the payload for the `user.checked-in.v1` event:

| Name | Type | Description |
| ---- | ---- | ----------- |
| customerToken | base64 string | A reference to the customer. Should be used when initiating a payment through the ePayment API. |
| merchantQrId | string | The ID of the QR code that has been scanned which is defined by the merchant when the QR was created. |
| merchantSerialNumber | string | A unique ID of the sales unit to which the scanned QR belongs. |
| initiatedAt | UTC Timestamp in ISO 8601 format | The timestamp of when the customer scanned the QR. |


Example of `user.checked-in.v1` payload:
````
{
    "customerToken": "wbA8ceVRKkoYiQAVELHeFCC3Sn5dtNCvvEtVPiOT77j6wx7uR965AG6Q+q0ATP4=",
    "merchantQrId": "d8b7d76d-49aa-48b8-90c6-38779372c163",
    "merchantSerialNumber": "12345",
    "initiatedAt": "2023-10-06T10:45:40.3061965Z"
}
````

See the [Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/) for more information.

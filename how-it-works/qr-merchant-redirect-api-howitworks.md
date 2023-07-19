<!-- START_METADATA
---
title: How the QR API works with merchant redirect
sidebar_label: Merchant redirect
sidebar_position: 10
description: How the QR API works with merchant redirect
pagination_next: null
pagination_prev: null
hide_table_of_contents: true
---
END_METADATA -->

# How the QR API works with merchant redirect

Offer contactless payment to your customers by creating interactions in static points of sales.

!["MerchantRedirect QR Flow"](../images/merchant-redirect-qr-flow.svg)

### 1. The merchant generates a QR code  
Generate the QR either via the QR-api or the merchant portal. The illustrations below shows how it is done in the portal.  

First find "**Skann & Vipps**" in the left menu
!["Find Skann & Vipps in the menu"](../images/skann_vipps_qr_gen_1.png)  

Then select the sales unit you want to use, and click the "**Lag ny QR-kode**"-button. Now you can enter the name you want for the QR, and enter the URL.
!["Choose a name and a URL for the QR"](../images/skann_vipps_qr_gen_2.png) Click "**Lag QR**" and the QR will be created.

The QR is now created, and you can download it in either SVG or PNG. You can also change the URL or delete the QR.
!["Download the QR or change the URL"](../images/skann_vipps_qr_gen_3.png)

### 2. The merchant places or publishes the QR code  
This will enable the merchants desired action, in a context.  

!["The merchant places the QR"](../images/skann_vipps_merchant_publish.png)

### 3. The user is interested and scans the QR code  
  
!["The user scans the QR"](../images/skann_vipps_user_scan.png)

### 4. The user is redirected to the merchant's page  
  
!["The user is redirected"](../images/skann_vipps_user_pays.png)

### 5. The user performs the merchant desired action. After payment, the order is confirmed at the merchant shop.
  
!["The user is redirected"](../images/skann_vipps_user_pays_2.png)













---
nav_title: Logging Purchases
platform: Unity
page_order: 3
description: "This reference article covers how to log purchases on Unity platform."

---

# Logging Purchases

Record in-app purchases so that you can track your revenue over time and across revenue sources, as well as segment your users by their lifetime value.

Braze supports purchases in multiple currencies. Purchases that you report in a currency other than USD will be shown in the dashboard in USD based on the exchange rate at the date they were reported.

Before implementation, be sure to review examples of the segmentation options afforded by custom events vs. custom attributes vs. purchase events in our [Best Practices section][5].

To use this feature, add this method call after a successful purchase in your app:

```csharp
AppboyBinding.LogPurchase("productId", "currencyCode", price(decimal));
```

The above method logs a purchase with a quantity of 1. If you would like to pass in a different quantity, you can call this method:

```csharp
AppboyBinding.LogPurchase("productId", "currencyCode", price(decimal), quantity(int));
```

Quantity must be less than or equal to 100. Braze also supports adding metadata about purchases by passing a `Dictionary` of purchase properties:

```csharp
AppboyBinding.LogPurchase("productId", "currencyCode", price(decimal), quantity(int), properties(Dictionary<string, object>));
```

## Currency Codes

Supported currency symbols are listed below. Any other provided currency symbol will result in a logged warning and no other action taken by the SDK.

- USD, AED, AFN, ALL, AMD, ANG, AOA, ARS, AUD, AWG, AZN, BAM, BBD, BDT, BGN, BHD, BIF, BMD, BND, BOB, BRL, BSD, BTC, BTN, BWP, BYR, BZD, CAD, CDF, CHF, CLF, CLP, CNY, COP, CRC, CUC, CUP, CVE, CZK, DJF, DKK, DOP, DZD, EEK, EGP, ERN, ETB, EUR, FJD, FKP, GBP, GEL, GGP, GHS, GIP, GMD, GNF, GTQ, GYD, HKD, HNL, HRK, HTG, HUF, IDR, ILS, IMP, INR, IQD, IRR, ISK, JEP, JMD, JOD, JPY, KES, KGS, KHR, KMF, KPW, KRW, KWD, KYD, KZT, LAK, LBP, LKR, LRD, LSL, LTL, LVL, LYD, MAD, MDL, MGA, MKD, MMK, MNT, MOP, MRO, MTL, MUR, MVR, MWK, MXN, MYR, MZN, NAD, NGN, NIO, NOK, NPR, NZD, OMR, PAB, PEN, PGK, PHP, PKR, PLN, PYG, QAR, RON, RSD, RUB, RWF, SAR, SBD, SCR, SDG, SEK, SGD, SHP, SLL, SOS, SRD, STD, SVC, SYP, SZL, THB, TJS, TMT, TND, TOP, TRY, TTD, TWD, TZS, UAH, UGX, UYU, UZS, VEF, VND, VUV, WST, XAF, XAG, XAU, XCD, XDR, XOF, XPD, XPF, XPT, YER, ZAR, ZMK, ZMW and ZWL.

## Sample Code

```csharp
AppboyBinding.LogPurchase("product ID", "USD", 12.5m);
```

## REST API

You can also use our REST API to record purchases. Refer to the [user API documentation][4] for details.

[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[5]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection

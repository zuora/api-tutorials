# Create Billing Documents
Billing documents represent your customer's invoices, credit memos, and debit memos.

Invoices are statements of amounts owed by a customer, and are either generated one-off or periodically from a subscription. They contain invoice items, and proration adjustments that may be caused by changes to a subscription.

Suppose you are trying to generate two invoices at the same time. 

In this guide, you will learn:
- How to create invoices by API
- How to post invoices by API
- How to create creditmemos by API
- How to post debitmemos by API

Note that to use this operation, you must have the "Create Standalone Invoice" and "Modify Invoice" user permissions. See [Billing Roles](https://knowledgecenter.zuora.com/CF_Users_and_Administrators/A_Administrator_Settings/User_Roles/d_Billing_Roles?_gl=1*1og0aqn*_ga*ODc2NzgzNzgyLjE2NjU5NzE1NzM.*_ga_21NV2LS8PH*MTY3MDQ2NTUyOC4yNi4xLjE2NzA0NjYxNzguMC4wLjA.&_ga=2.124706634.1132406548.1670221612-876783782.1665971573) for more information. As of Zuora Release 2022.03.R5, newly created standard Billing users have the “Create Standalone Invoice” permission enabled by default.

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the Create Standalone Invoices API.

*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/invoices/batch' \
--header 'Authorization: Bearer 476c8614d7fe4aa78de9fbc20f49cea4' \
--header 'Content-Type: application/json' \
--data-raw '{
    "invoices": [
        {
            "accountId": "8a90876c84ae5ef70184c6c45f6f69fb",
            "autoPay": false,
            "comments": "comments",
            "invoiceDate": "2022-10-01",
            "invoiceItems": [
                {
                    "amount": 100,
                    "bookingReference": "bookingReference",
                    "chargeDate": "2022-10-01 00:00:00",
                    "description": "description",
                    "discountItems": [
                        {
                            "amount": -10,
                            "bookingReference": "discountBookingReference",
                            "chargeDate": "2022-10-01 11:00:00",
                            "chargeName": "discount",
                            "description": "description",
                            "sku": "SKU-0002",
                            "taxItems": [
                                {
                                    "exemptAmount": 0,
                                    "jurisdiction": "jurisdiction",
                                    "locationCode": "locationCode",
                                    "name": "country tax",
                                    "taxAmount": -1,
                                    "taxCode": "country tax code",
                                    "taxCodeDescription": "country tax code, tax rate 10%",
                                    "taxDate": "2022-10-01",
                                    "taxMode": "TaxExclusive",
                                    "taxRate": 0.1,
                                    "taxRateDescription": "country tax",
                                    "taxRateType": "Percentage"
                                }
                            ]
                        }
                    ],
                    "productRatePlanChargeId": "ff8080817cda56fa017cda87999d071b",
                    "purchaseOrderNumber": "PO-000303",
                    "quantity": 1,
                    "serviceEndDate": "2022-10-12",
                    "serviceStartDate": "2022-10-01",
                    "taxItems": [
                        {
                            "exemptAmount": 0,
                            "jurisdiction": "juristiction",
                            "locationCode": "locationCode",
                            "name": "country tax",
                            "taxAmount": 10,
                            "taxCode": "tax code",
                            "taxCodeDescription": "tax code description",
                            "taxDate": "2020-02-01",
                            "taxMode": "TaxExclusive",
                            "taxRate": 0.01,
                            "taxRateDescription": "tax rate description",
                            "taxRateType": "Percentage"
                        }
                    ]
                },
                {
                    "amount": 100,
                    "bookingReference": "bookingReference",
                    "chargeDate": "2022-10-01 00:00:00",
                    "chargeName": "charge name",
                    "description": "description",
                    "purchaseOrderNumber": "PO-000303",
                    "quantity": 1,
                    "serviceEndDate": "2022-10-12",
                    "serviceStartDate": "2022-10-01",
                    "sku": "sku-001",
                    "uom": "each"
                }
            ]
        },
        {
            "accountId": "8a90876c84ae5ef70184c6c45f6f69fb",
            "autoPay": false,
            "comments": "comments",
            "invoiceDate": "2022-10-01",
            "invoiceItems": [
                {
                    "amount": 100,
                    "bookingReference": "bookingReference",
                    "chargeDate": "2022-10-01 00:00:00",
                    "description": "description",
                    "productRatePlanChargeId": "ff8080817cda56fa017cda87999d071b",
                    "purchaseOrderNumber": "PO-000303",
                    "quantity": 1,
                    "serviceEndDate": "2022-10-12",
                    "serviceStartDate": "2022-10-01",
                    "taxItems": [
                        {
                            "exemptAmount": 0,
                            "jurisdiction": "juristiction",
                            "locationCode": "locationCode",
                            "name": "country tax",
                            "taxAmount": 10,
                            "taxCode": "tax code",
                            "taxCodeDescription": "tax code description",
                            "taxDate": "2022-10-01",
                            "taxMode": "TaxExclusive",
                            "taxRate": 0.01,
                            "taxRateDescription": "tax rate description",
                            "taxRateType": "Percentage"
                        }
                    ]
                },
                {
                    "amount": 100,
                    "bookingReference": "bookingReference",
                    "chargeDate": "2022-10-12 00:00:00",
                    "chargeName": "charge name",
                    "description": "description",
                    "purchaseOrderNumber": "PO-000303",
                    "quantity": 1,
                    "serviceEndDate": "2022-10-12",
                    "serviceStartDate": "2022-10-01",
                    "sku": "sku-001",
                    "uom": "each"
                }
            ]
        }
    ],
    "useSingleTransaction": false
}'
```

#### Note that this operation has the following limitations:

 - You can create a maximum of 50 invoices in one request.
 - You can create a maximum of 1,000 invoice items in one request.

## Step 3. Submit a API request to the Post Invoices API.

You may find the invoices ID from the response payload of Step 2.

*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/invoices/bulk-post' \
--header 'Authorization: Bearer 476c8614d7fe4aa78de9fbc20f49cea4' \
--header 'Content-Type: application/json' \
--data-raw '{
  "invoices": [
    {
      "id": "402890555a7e9791015a7f15fe440123",
      "invoiceDate": "2022-10-12"
    },
    {
      "id": "402890555a7e9791015a7f15fe44013a",
      "invoiceDate": "2022-10-12"
    }
  ]
}'
```

## Step 4. Create creditmemo or a debitmemo from an invoice (optional)
Credit memos reduce invoice and account balances. By applying one or more credit memos to invoices with positive balances, you can reduce the invoice balances in the same way that applying a payment to an invoice.

Debit memos increase the amount a customer owes. It is a separate document from the invoice. Debit memos can be used to correct undercharging on an invoice or to levy ad hoc charges outside the context of a subscription. Just like an invoice, debit memo balances can be settled by applying either a payment or a credit memo.

This step might be useful when you find there should be some adjustments for the invoice amount.

#### Credit Memo

*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/invoices/402890555a7e9791015a7f15fe440123/creditmemos' \
--header 'Authorization: Bearer 476c8614d7fe4aa78de9fbc20f49cea4' \
--header 'Content-Type: application/json' \
--data-raw '{
  "autoApplyToInvoiceUponPosting": false,
  "autoPost": false,
  "comment": "the comment",
  "effectiveDate": "2022-10-12",
  "excludeFromAutoApplyRules": false,
  "items": [
    {
      "amount": 1,
      "comment": "This is comment!",
      "invoiceItemId": "4028905558b483220158b48983dd0015",
      "quantity": 1,
      "serviceEndDate": "2022-10-12",
      "serviceStartDate": "2022-10-01",
      "skuName": "sku-001",
      "taxItems": [
        {
          "amount": 0.01,
          "jurisdiction": "CALIFORNIA",
          "locationCode": "06",
          "sourceTaxItemId": "4028905558b483220158b48983150010",
          "taxCode": null,
          "taxCodeDescription": "This is tax code description!",
          "taxDate": "2022-10-01",
          "taxExemptAmount": 0,
          "taxName": "STATE TAX",
          "taxRate": 0.0625,
          "taxRateDescription": "This is tax rate description!",
          "taxRateType": "Percentage"
        }
      ],
      "taxMode": "TaxExclusive",
      "unitOfMeasure": "Test_UOM"
    }
  ],
  "reasonCode": "Write-off"
}'
```

#### Debit Memo

*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/invoices/402890555a7e9791015a7f15fe440123/debitmemos' \
--header 'Authorization: Bearer 476c8614d7fe4aa78de9fbc20f49cea4' \
--header 'Content-Type: application/json' \
--data-raw '{
  "autoPay": true,
  "comment": "the comment",
  "effectiveDate": "2017-11-30",
  "items": [
    {
      "amount": 1,
      "comment": "This is comment!",
      "invoiceItemId": "402890555a7d4022015a7dadb3b700a6",
      "quantity": 1,
      "serviceEndDate": "2017-11-30",
      "serviceStartDate": "2017-11-01",
      "skuName": "SKU-30",
      "taxItems": [
        {
          "amount": 0.01,
          "jurisdiction": "CALIFORNIA",
          "locationCode": "06",
          "sourceTaxItemId": "402890555a7d4022015a7dadb39b00a1",
          "taxCode": null,
          "taxCodeDescription": null,
          "taxDate": "2017-11-30",
          "taxExemptAmount": 0,
          "taxName": "STATE TAX",
          "taxRate": 0.0625,
          "taxRateDescription": "This is tax rate description!",
          "taxRateType": "Percentage"
        }
      ],
      "taxMode": "TaxExclusive",
      "unitOfMeasure": "Test_UOM"
    }
  ],
  "reasonCode": "Charge Dispute"
}'
```

## Step 5. Verify the result
After the above order actions are done, you can verify the result in the Zuora UI or through the GET API operations for the Invoices/Credit Memos/Debit Memos.

You can search the invoices by ID at the All Invoices page by navigating to **Billing** > **Invoices** in the Zuora UI.

You can also search the credit memo and debit memo by ID at the All Credit Memos or All Debit Memos page by navigating to **Billing** > **Credit and Debit Memos** in the Zuora UI.

To verify the invoicing result through the API, See [Retrieve Object Info](9-retrieve-object-info.md) for details.
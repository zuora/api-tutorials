# Create a Payment
The Payment object holds all of the information about a payment, including the payment amount and to which billing documents the payment is applied.

You can create a new payment to apply a payment to one or more invoices. 

Suppose you already have the invoice numbers retrieved from the [step](7-billing-document.md) above. Now, you are trying to create an external payment and close this transaction. You will need a payment with 44.1 USD as the total amount. Both the debit memos amount and the invoices balance should be considered and paied.

In this guide, you will learn:
- How to create a payment through API.

Note: This operation is only available if you have Invoice Settlement enabled. The Invoice Settlement feature is generally available as of Zuora Billing Release 296 (March 2021). This feature includes Unapplied Payments, Credit and Debit Memo, and Invoice Item Settlement. If you want to enable Invoice Settlement, see Invoice Settlement Enablement and Checklist Guide for more information.

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the prodcut creation API

*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/payments' \
--header 'Authorization: Bearer 5b54ae5dce244c3cbb26e781dc27c703' \
--header 'Content-Type: application/json' \
--data-raw '{
  "accountId": "4028905f5a87c0ff015a87d25ae90025",
  "amount": 44.1,
  "comment": "normal payment",
  "currency": "USD",
  "customRates": [
    {
      "currency": "CAD",
      "customFxRate": 2.22,
      "rateDate": "2022-10-21"
    },
    {
      "currency": "EUR",
      "customFxRate": 1.5,
      "rateDate": "2022-10-21"
    }
  ],
  "debitMemos": [
    {
      "amount": 4.1,
      "debitMemoId": "4028905f5a87c0ff015a87e49e6b0062",
      "items": [
        {
          "amount": 4,
          "debitMemoItemId": "4028905f5a87c0ff015a87e49e7a0063"
        },
        {
          "amount": 0.1,
          "taxItemId": "4028905f5a87c0ff015a87e49f5e0065"
        }
      ]
    }
  ],
  "effectiveDate": "2022-12-01",
  "invoices": [
    {
      "amount": 40,
      "invoiceId": "4028905f5a87c0ff015a87d3f8f10043",
      "items": [
        {
          "amount": 39,
          "invoiceItemId": "4028905f5a87c0ff015a87d3f90c0045"
        },
        {
          "amount": 1,
          "taxItemId": "4028905f5a87c0ff015a87d3f884003f"
        }
      ]
    }
  ],
  "paymentMethodId": "402881e522cf4f9b0122cf5dc4020045",
  "type": "External"
}'
```

## Step 3. Verify the result
After the product is created, you can verify the result in the Zuora UI or through the GET API operations for the Payment object.

To verify the result through the Zuora UI, you can find the payment displayed at the top of the All Payments page by navigating to **Payments** > **Payments** in the Zuora UI.

To verify the result through the API, See [Retrieve Object Info](9-retrieve-object-info.md) for details.

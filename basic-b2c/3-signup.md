# Sign up Customers

## Case description
Suppose that a new customer wants to subscribe to a product offered by you. You need to set the following things up for the customer:

1. The subscriber creates a new account or sign in to their account
2. The subscriber types in payment method on the checkout page
3. The provider auths the card details and returns a reference code
4. The provider sends the request (account ID, customer details, payment reference code, contact, plan to subscribe) to Zuora
5. Zuora handle the subscription flow and return the sign-up result back to the provider.

In this guide, you will learn:
- How to set up an account for new customers, create a subscription, create invoice if needed and capture the payment if auth id is provided.


Note that this easy sign-up API is scenario based, and don't use it if the subscribers want to:
- Only create accounts and do not need to subscribe to any products right away. Please find instructions in [Create an Acount](3.1-create-account.md).
- Only create payment methods for an existing account. Please find instructions in [Create payment methods](3.2-create-paymentmethod.md).
- Only create subscriptions for an existing account. Please find instructions in [Create subscriptions](3.3-create-subscription.md)


## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the sign-up API
*API Sample payload with curl*
```
curl --location --request POST 'https://rest.zuora.com/v1/sign-up' \
--header 'Authorization: Bearer ea5e5d28d03a4ed2892d4c22a80e858d' \
--header 'Content-Type: application/json' \
--data-raw '{
  "accountData": {
    "accountNumber": "John",
    "name": "Smith",
    "currency": "USD",
    "notes": "Online Payment",
    "billCycleDay": 31,
    "crmId": "SAP-LSCRM-01111",
    "autoPay": true,
    "billToContact": {
      "address1": "1051 E Hillsdale Blvd",
      "city": "Foster City",
      "country": "United States",
      "firstName": "John",
      "lastName": "Smith",
      "state": "CA",
      "workEmail": "smith@example.com",
      "zipCode": "94404"
      "otherPhone": "",
      "otherPhoneType": "Mobile",
      "personalEmail": "user@example.com",
      "postalCode": "00000",
      "taxRegion": "",
      "workPhone": ""
    },
    "soldToContact": {
      "address1": "1051 E Hillsdale Blvd",
      "city": "Foster City",
      "country": "United States",
      "firstName": "John",
      "lastName": "Smith",
      "state": "CA",
      "workEmail": "smith@example.com",
      "zipCode": "94404"
      "otherPhone": "",
      "otherPhoneType": "Mobile",
      "personalEmail": "user@example.com",
      "postalCode": "00000",
      "taxRegion": "",
      "workPhone": ""
    },
     "paymentMethod": {
      "type": "PayPalEC"
    }
  },
  "subscriptionData": {
    "subscriptionNumber": "SUB-00000001",
    "ratePlans": [
      {
        "productRatePlanId": "8a90a9b784ae5f180184c6c4650446a1"
      }
    ],
    "terms": {
      "initialTerm": {
        "startDate": "2022-11-29",
        "period": 0,
        "periodType": "Month",
        "termType": "TERMED"
      },
      "renewalTerms": [
        {
          "period": 0,
          "periodType": "Month"
        }
      ],
      "renewalSetting": "RENEW_WITH_SPECIFIC_TERM",
      "autoRenew": true
    },
    "startDate": "2022-11-29",
    "invoiceSeparately": true,
    "notes": ""
  },
  "accountIdentifierField": "1",
  "options": {
    "runBilling": true,
    "collectPayment": true,
    "billingTargetDate": "2022-11-29",
    "maxSubscriptionsPerAccount": 0
  }
}'
```
## Step 3. Verify the result
After the sign-up process is done, you can verify the result in the Zuora UI or through the GET API operations for the Account object and Subscription obkect.
To verify the result through the Zuora UI, you can find the created account displayed at the top of the All Customer Accounts page by navigating to Customers > Customer Accounts in the Zuora UI.
To verify the result through the API, See [Retrieve Object Info](9-retrieve-object-info.md) for details.


# Cancel Subscriptions with Auto Refunds
In this use case, the end customer A has a termed subscription with contract effective date from 2022-01-01. He made a payment with $1100 already for the current term. An invoice has already been generated for the customer, with the amount of $1100. He canceled the subscription in the middle of the current term and should get a $700 refund based on the time and usage, but he claimed that he is not satisfied with the service. After investigating this case, the service provider decided to refund an additional $100 to the customer.


In this guide, you will learn:
- How to cancel a subscription with refund by API.

**Note**: Only active subscriptions could be cancelled.

## Step 1. Generate an OAuth token

See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Call the order creation API with Cancel a Subscription order action
```
curl --location --request POST 'https://rest.zuora.com/v1/orders' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic e3tiYnlfZml4X3VzZXJuYW1lfX06e3tiYnlfZml4X3Bhc3N3b3JkfX0=' \
--data-raw '{
    "orderDate": "2022-04-30",
    "existingAccountNumber": "A%00026428",
    "subscriptions": [
        {
            "subscriptionNumber": "A-S00020635",
            "orderActions": [
                {
                    "type": "CancelSubscription",
                    "triggerDates": [
                        {
                            "name": "CustomerAcceptance",
                            "triggerDate": "2022-04-30"
                        },
                        {
                            "name": "ServiceActivation",
                            "triggerDate": "2022-04-30"
                        },
                        {
                            "name": "ContractEffective",
                            "triggerDate": "2022-04-30"
                        }
                    ],
                    "cancelSubscription": {
                        "cancellationPolicy": "SpecificDate",
                        "cancellationEffectiveDate": "2022-04-30"
                    }
                }
            ]
        }
    ],
    "processingOptions": {
        "runBilling": true,
        "billingOptions": {
            "documentDate": "2022-04-30",
            "targetDate": "2022-04-30"
        },
        "refund": true,
        "refundAmount": 700,
        "writeOff": true
    }
}'
```
## Step 3. Generate a refund by electronic referenced refund automatically by Zuora.

In this case, the refund will be splite into several parts:
1. Generate a credit memo (CM-0001) for -$700 (Based on the automatic calculation with his original term and payment).
2. Unapply $700 from P-0001 (this will cause INV001 to have an open balance of $700).
3. Refund $700 from unapplied balance of P-001 by sending electronic referenced refund request (R-0001) to the payment gateway.
4. Create a standalone credit memo (CM-002) for the remaining balance of $100 for INV001. 

Although it looks complicated, don't worry, Zuora's API will take it over for you!

## Step 4. Verify the result

After the process is done, you can verify the result in the Zuora UI or through the GET API operations for the Orders.
To verify the result through the Zuora UI, you can find the Subscription you canceled displayed at the top of the All Subscriptions page by navigating to **Customers** > **Subscription** in the Zuora UI. You will find the current status of this subscription is canceled already, with two credit memos generated in **Associated Events** > **Credit Memos**.

To verify the result through the API, See [Retrieve Object Info](9-retrieve-object-info.md) for details.
# Update Prices on Subscriptions with Notifications
Company A’s rate plan needs to be adjusted frequently because of the business needs, and wants to keep their customers notified by email during every upgrade/downgrade. They have an API to spreed the notification. What they need to do is, every time the rate plan is changed, Zuora could call the external API immediately to acknowledge the customers.

In this guide, you will learn:
- How to update a price by API
- How to trigger and send notifications by API

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the order creation API
When an end subscriber wants to upgrade or downgrade a product for a subscription, two order actions are required in the single [Create an order](https://www.zuora.com/developer/api-references/api/operation/POST_Order/) operation. One action is to remove the previous rate plan from the subscription, and the other is to add the new rate plan.

In this scenario, both `removeProduct` and `addProduct` actions are used in the "Create an order" operation. You will need to provide the following information in the API request:
  - `orderDate`: The date when the order is signed.
  - `existingAccountNumber`: The number of the account that owns this order.
  - `subscriptionNumber`: The number of the existing subscription to be updated.

The following fields are required for the `removeProduct` action:
  - `contractEffective`: The contract effective date of product removal.
  - `serviceActivation`: The service activation date of product removal.
  - `customerAcceptance`: The end subscriber acceptance date of product removal.
  - `ratePlanId`: The ID of the rate plan to be removed from the subscription.

The following fields are required for the `addProduct` action:
  - `contractEffective`: The contract effective date of the product addition.
  - `serviceActivation`: The service activation date of the product addition.
  - `customerAcceptance`: The end subscriber acceptance date of the product addition.
  - `productRatePlanId`: The ID of the product rate plan to add to the subscription.

Make an order creation API call with the instructions below, the upgrade/downgrade should be done.

```
 curl --location --request POST 'https://rest.zuora.com/v1/orders' \
      --header 'Authorization: Bearer 0228427979714ca690ff07d4d648ac21' \
      --header 'Content-Type: application/json' \
      --data-raw '{
        "description": "Price update by removing and adding products",
        "existingAccountNumber": "A00000521",
        "orderDate": "2022-12-01",
        "subscriptions": [

          {
            "orderActions": [
              {
                "removeProduct": {
                  "ratePlanId": "8a90a9b784ae5f180184c6c46a1a46b6"
                },
                "triggerDates": [
                  {
                    "name": "ServiceActivation",
                    "triggerDate": "2023-03-01"
                  },
                  {
                    "name": "CustomerAcceptance",
                    "triggerDate": "2023-04-01"
                  }
                ],
                "type": "RemoveProduct"
              }
            ]
          },
          {
            "orderActions": [
              {
                "addProduct": {
                  "productRatePlanId": "8a90a9b784ae5f180184c6c46a1a46b6"
                },
                "triggerDates": [
                  {
                    "name": "ContractEffective",
                    "triggerDate": "2023-04-01"
                  },
                  {
                    "name": "ServiceActivation",
                    "triggerDate": "2023-05-01"
                  },
                  {
                    "name": "CustomerAcceptance",
                    "triggerDate": "2023-05-01"
                  }
                ],
                "type": "AddProduct"
              }
            ],
            "subscriptionNumber": "A-S00000272"
          }
        ]
      }'
```

## Step 3. Create a custom event trigger by requesting the event-trigger API 
See: [Create an event trigger](https://www.zuora.com/developer/api-references/api/operation/POST_EventTrigger/). 
Make sure to set the condition field correctly.

## Step 4. Create a notification definition through API
Use the [Create a notification definition](https://www.zuora.com/developer/api-references/api/operation/POST_Create_Notification_Definition/) operation to create notification definitions. Simply set the `callout` > `calloutBaseurl` field to the url that you want to callout, then you are all set.

## Step 5. Verify the result by checking the notification information

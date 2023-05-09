# Renew, Suspend, or Resume Subscriptions
When a subscription expires or before it expires, you can renew the subscription. 

If the end subscriber requests to suspend or pause a subscription, use the Pause a subscription operation to do it.

When a subscription is in the Suspended status, the only action you can take is to resume a suspended subscription with the Resume a subscription operation.

In this guide, you will learn:
- How to renew a subscription by API
- How to suspend a subscription by API
- How to resume a subscription by API

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the order creation API, with renewSubscription/resume/suspend order actions.

### For suspending a subscription
```
curl --location --request POST 'https://rest.zuora.com/v1/orders' \
--header 'Authorization: Bearer 39cdc89ece8741fb97b8a2477622bc09' \
--header 'Content-Type: application/json' \
--data-raw '{
  "description": "Suspend a subscriptions",
  "existingAccountNumber": "A00000521",
  "orderDate": "2023-01-01",
  "subscriptions": [
    {
      "orderActions": [
        {
          "suspend": {
            "suspendPolicy": "Today"
          },
          "triggerDates": [
            {
              "name": "ContractEffective",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "ServiceActivation",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "CustomerAcceptance",
              "triggerDate": "2018-01-01"
            }
          ],
          "type": "Suspend"
        }
      ],
      "subscriptionNumber": "A-S00000272"
    }
  ]
}'
```

### For resuming a subscription
```
curl --location --request POST 'https://rest.zuora.com/v1/orders' \
--header 'Authorization: Bearer 39cdc89ece8741fb97b8a2477622bc09' \
--header 'Content-Type: application/json' \
--data-raw '{
  "description": "Resume a subscriptions",
  "existingAccountNumber": "A00000521",
  "orderDate": "2023-01-01",
  "subscriptions": [
    {
      "orderActions": [
        {
          "resume": {
            "extendsTerm": true,
            "resumePolicy": "SpecificDate",
            "resumeSpecificDate": "2018-10-01"
          },
          "triggerDates": [
            {
              "name": "ContractEffective",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "ServiceActivation",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "CustomerAcceptance",
              "triggerDate": "2018-01-01"
            }
          ],
          "type": "Resume"
        }
      ],
      "subscriptionNumber": "A-S00000272"
    }
  ]
}'
```

### For subscription renewal
```
curl --location --request POST 'https://rest.zuora.com/v1/orders' \
--header 'Authorization: Bearer 39cdc89ece8741fb97b8a2477622bc09' \
--header 'Content-Type: application/json' \
--data-raw '{
  "description": "resume a subscriptions",
  "existingAccountNumber": "A00000521",
  "orderDate": "2023-01-01",
  "subscriptions": [
    {
      "orderActions": [
        {
          
          "triggerDates": [
            {
              "name": "ContractEffective",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "ServiceActivation",
              "triggerDate": "2018-01-01"
            },
            {
              "name": "CustomerAcceptance",
              "triggerDate": "2018-01-01"
            }
          ],
          "type": "RenewSubscription"
        }
      ],
      "subscriptionNumber": "A-S00000272"
    }
  ]
}'
```

### Some other options
You may also use the subscription APIs for renewing, suspending or resuming a subscription if you only have one sigle action each time.

#### For subscription renewal
```
curl -i -X PUT \
  'https://rest.zuora.com/v1/subscriptions/{subscription-key}/renew' \
  -H 'Accept-Encoding: string' \
  -H 'Authorization: string' \
  -H 'Content-Encoding: string' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -H 'Idempotency-Key: string' \
  -H 'Zuora-Entity-Ids: string' \
  -H 'Zuora-Track-Id: string' \
  -H 'zuora-version: string' \
  -d '{
    "collect": false,
    "creditMemoReasonCode": "Unsatisfactory service",
    "runBilling": true
  }'
```
#### For resuming a subscription
```
curl -i -X PUT \
  'https://rest.zuora.com/v1/subscriptions/{subscription-key}/resume' \
  -H 'Accept-Encoding: string' \
  -H 'Authorization: string' \
  -H 'Content-Encoding: string' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -H 'Idempotency-Key: string' \
  -H 'Zuora-Entity-Ids: string' \
  -H 'Zuora-Track-Id: string' \
  -H 'zuora-version: string' \
  -d '{
    "collect": false,
    "contractEffectiveDate": "2019-02-01",
    "creditMemoReasonCode": "Unsatisfactory service",
    "extendsTerm": true,
    "resumePolicy": "SpecificDate",
    "resumeSpecificDate": "2019-10-01",
    "runBilling": true
  }'
```
#### For suspending a subscription
```
curl -i -X PUT \
  'https://rest.zuora.com/v1/subscriptions/{subscription-key}/suspend' \
  -H 'Accept-Encoding: string' \
  -H 'Authorization: string' \
  -H 'Content-Encoding: string' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -H 'Idempotency-Key: string' \
  -H 'Zuora-Entity-Ids: string' \
  -H 'Zuora-Track-Id: string' \
  -H 'zuora-version: string' \
  -d '{
    "collect": false,
    "contractEffectiveDate": "2019-02-01",
    "creditMemoReasonCode": "Unsatisfactory service",
    "extendsTerm": true,
    "resume": true,
    "resumePolicy": "SpecificDate",
    "resumeSpecificDate": "2019-06-01",
    "runBilling": true,
    "suspendPeriods": 10,
    "suspendPeriodsType": "Day",
    "suspendPolicy": "FixedPeriodsFromToday"
  }'
```

## Step 3. Verify the result
After the above order actions are done, you can verify the result in the Zuora UI.
To verify the result through the Zuora UI, you can search the subscription by ID at the All Subscriptions page by navigating to Customers > Subscriptions in the Zuora UI.
By clicking the subscription, you will be able to find the status of it, and you may verify if the subscription has been suspended/resumed/renewed or not.


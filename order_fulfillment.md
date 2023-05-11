# Basic Order line item with fulfillment API Tutorial
Order line items are non-subscription based items created by an order, representing transactional charges such as one-time fees, physical goods, or professional service charges that are not sold as subscription services. With the Order Line Items feature enabled, customers now have the ability to launch non-subscription and unified monetization business models in Zuora, in addition to subscription business models. And you can integrate Fulfillment with Zuora, e.g. add fulfillments to existing order line items that have been activated. 

By following this tutorial, you will learn how to build a classic basic fulfillment integration leveraging the Zuora REST API.

## Prerequisites

- You must have the following features enabled on your tenant:
  - **Orders**. If this feature is enabled in your tenant, you can see **Orders** under **Customers** in the left navigation panel in the Zuora UI. To obtain access to this feature, submit a request at <a href="https://support.zuora.com/" target="_blank">Zuora Global Support</a>.
  - **Invoice Settlement**. If this feature is enabled in your tenant, you can see **Credit and Debit Memos** under **Billing** in the left navigation panel in the Zuora UI. To obtain access to this feature, submit a request at <a href="https://support.zuora.com/" target="_blank">Zuora Global Support</a>.

- You must have an OAuth client created. See <a href="https://knowledgecenter.zuora.com/Billing/Tenant_Management/A_Administrator_Settings/Manage_Users#Create_an_OAuth_Client_for_a_User" target="_blank">Create an OAuth Client for a User</a> for more information.

## Step 1. Generate an OAuth token
See [Create an OAuth token](./basic-b2c/1-authentication.md) for details.  

Get the token and replace `{{bearer_token}}` in this doc with the actual value.

## Step 2. (Optional) Create a demo billing account for the end user if it does not exist.

``` 
curl --location --request POST 'https://rest.zuora.com/v1/accounts' \
--header 'Authorization: Bearer {{bearer_token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "additionalEmailAddresses": [
        "contact1@example.com",
        "contact2@example.com"
    ],
    "autoPay": false,
    "billCycleDay": 0,
    "billToContact": {
        "address1": "1051 E Hillsdale Blvd",
        "city": "Foster City",
        "country": "United States",
        "firstName": "John",
        "lastName": "Smith",
        "state": "CA",
        "workEmail": "smith@example.com",
        "zipCode": "94404"
    },
    "currency": "USD",
    "name": "Zuora Fulfillment Account",
    "notes": "This account is for demo purposes."
}' 
``` 

The response body looks like 
```
{
    "success": true,
    "accountId": "8a90f50888092efb0188097c364511d2",
    "accountNumber": "A00082003",
    "billToContactId": "8a90f50888092efb0188097c367311d3",
    "soldToContactId": "8a90f50888092efb0188097c368311d5"
}
```

## Step 3. Create an order with a non-subscription order line item and generate an invoice.
In this step, the end user bought 10 physical goods with each unit priced at $30. An invoice is generated automatically with the total amount of $300.  

Need to replace `{{accountNumber}}` with the returned value from step2. In this tutorial, it is `A00082003`.

``` 
curl --location --request POST 'https://rest.zuora.com/v1/orders?returnIds=true' \
--header 'Authorization: Bearer {{bearer_token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "orderDate": "2021-01-01",
    "existingAccountNumber": "{{accountNumber}}",
    "orderLineItems": [
        {
            "itemName": "physical good _fbff79b9",
            "itemType": "Product",
            "description": "physical good to sell",
            "purchaseOrderNumber": "PO-12345678",
            "productCode": "P-0000001",
            "quantity": 10,
            "amountPerUnit": 30,
            "listPricePerUnit": 30,
            "UOM": "Each",
            "transactionDate": "2021-01-01",
            "billTargetDate": "2021-01-01",
            "itemState": "SentToBilling"
        }
    ],
    "processingOptions": {
        "runBilling": true,
        "billingOptions": {
            "targetDate": "2021-01-01",
            "documentDate": "2021-01-01"
        }
    }
}' 
``` 

The response body looks like 
```
{
    "success": true,
    "orderNumber": "O-00170371",
    "accountNumber": "A00082003",
    "status": "Completed",
    "orderId": "8a90f50888092efb0188097c379511d8",
    "accountId": "8a90f50888092efb0188097c364511d2",
    "orderLineItems": [
        {
            "itemNumber": "1",
            "id": "8a90f50888092efb0188097c37db11da"
        }
    ],
    "invoiceNumbers": [
        "INV10208-00088595"
    ],
    "invoiceIds": [
        "8a90f50888092efb0188097c382a11dd"
    ]
}
```


## Step 3. API to return the goods, and track the return shipment.
In this step, the end user returned 2 physical goods. Set the billing rule to "TriggerAsFulfillmentOccurs" so no credit memo will be generated immediately until the fulfillment has been completed.  
More details can be found [here](https://knowledgecenter.zuora.com/Zuora_Billing/Subscriptions/Order_Line_Items/B_Manage_Order_Line_Items/AA_Create_a_sales_order_line_item_with_fulfillments)

Need to replace 
- `{{accountNumber}}` with the returned value from step2, in this tutorial it is `A00082003`. 
- `{{orderNumber}}` with the returned value from step3, in this tutorial it is `O-00170371`.
- `{{itemNumber}}` with the returned value from step3, in this tutorial it is `1` in orderLineItems.itemNumber.

``` 
curl --location --request POST 'https://rest.zuora.com/v1/orders?returnIds=true' \
--header 'Authorization: Bearer {{bearer_token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "orderDate": "2021-02-01",
    "category": "Return",
    "reasonCode": "No Longer Needed",
    "existingAccountNumber": "{{accountNumber}}",
    "description": "return device",
    "processingOptions": {
        "runBilling": false
    },
    "orderLineItems": [
        {
            "itemName": "return device detail",
            "itemState": "Booked",
            "itemNumber": "1",
            "itemCategory": "Return",
            "originalOrderNumber": "{{orderNumber}}",
            "originalOrderLineItemNumber": "{{itemNumber}}",
            "description": "With Dual Stereo Microphones.",
            "quantity": 2,
            "billTargetDate": "2021-02-01",
            "transactionDate": "2021-02-01",
            "transactionStartDate": "2021-02-01",
            "transactionEndDate": "2021-02-01",
            "billingRule": "TriggerAsFulfillmentOccurs"
        }
    ]
}' 
``` 

The response body looks like 

```
{
    "success": true,
    "orderNumber": "RMA-00170372",
    "accountNumber": "A00082003",
    "status": "Completed",
    "orderId": "8a90f50888092efb0188097c3a1511e6",
    "accountId": "8a90f50888092efb0188097c364511d2",
    "orderLineItems": [
        {
            "itemNumber": "1",
            "id": "8a90f50888092efb0188097c3a7c11e8"
        }
    ]
}
```


## Step 4. Add fulfillment to the existing return order line item and generate credit memos.
In this step, the two fulfillments with state "SentToBilling" are added to the returned order line item.

You can set different state for the fulfillment. The state of the fulfillment are Executing, Booked, Sent To Billing, Complete, and Canceled. The default value is Executing.
- If the fulfillment is in the Executing state, you can update the fulfillment.
- If the fulfillment is specified to other states, you can add fulfillment items to the fulfillment. To generate billing documents, you need to specify the fulfillment to the Sent To Billing state and set the Billing Target Date field.

More details can be found [here](https://knowledgecenter.zuora.com/Zuora_Billing/Subscriptions/Order_Line_Items/C_Fulfillments/Add_fulfillments_to_order_line_items)

Need to replace `{{returnOrderLineItemId}}` with the returned value from step3, in this tutorial it is `8a90f50888092efb0188097c3a7c11e8` in orderLineItems[0].id. 
``` 
curl --location --request POST 'https://rest.zuora.com/v1/fulfillments' \
--header 'Authorization: Bearer {{bearer_token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "fulfillments": [
        {
            "fulfillmentDate": "2021-02-01",
            "quantity": 2,
            "state": "SentToBilling",
            "billTargetDate": "2021-02-01",
            "orderLineItemId": "{{returnOrderLineItemId}}",
            "fulfillmentType": "Return"
        }
    ],
    "processingOptions": {
        "runBilling": true,
        "billingOptions": {
            "documentDate": "2021-02-01",
            "targetDate": "2021-02-01"
        }
    }
}' 
``` 

The response body looks like 
```
{
    "fulfillments": [
        {
            "id": "8a90876c880945ab0188097c3b790afa",
            "fulfillmentNumber": "F-00007053",
            "fulfillmentItems": []
        }
    ],
    "invoiceNumbers": null,
    "creditMemoNumbers": [
        "CM10208-00007761"
    ],
    "paymentNumber": null,
    "paidAmount": null,
    "success": true
}
```

## Step 5. Retrieve the credit memo generated for the account
In this step, we verify the generated credit memo for the account with total amount $60, $30 for each of the 2 returned devices.

Need to replace `{{accountNumber}}` with the returned value from step2, in this tutorial it is `A00082003`. 
``` 
curl --location --request GET 'https://rest.zuora.com/v1/creditmemos?accountNumber={{accountNumber}}' \
--header 'Authorization: Bearer {{bearer_token}}' \
--header 'Content-Type: application/json'  
``` 

The response body looks like 
```
{
    "creditmemos": [
        {
            "id": "8a90876c880945ab0188097c3bc90afd",
            "number": "CM10208-00007761",
            "accountId": "8a90f50888092efb0188097c364511d2",
            "accountNumber": "A00082003",
            "currency": "USD",
            "creditMemoDate": "2021-02-01",
            "targetDate": "2021-02-01",
            "postedById": "2c92c8fe78a9a0740178a9b0b06c007a",
            "postedOn": "2023-05-11 14:25:16",
            "status": "Posted",
            "amount": 60,
            "taxAmount": 0,
            "totalTaxExemptAmount": 0,
            "unappliedAmount": 60,
            "refundAmount": 0,
            "appliedAmount": 0,
            "comment": null,
            "source": "API",
            "sourceId": null,
            "referredInvoiceId": null,
            "reasonCode": "Return Order",
            "createdDate": "2023-05-11 14:25:16",
            "createdById": "2c92c8fe78a9a0740178a9b0b06c007a",
            "updatedDate": "2023-05-11 14:25:16",
            "updatedById": "2c92c8fe78a9a0740178a9b0b06c007a",
            "cancelledOn": null,
            "cancelledById": null,
            "latestPDFFileId": null,
            "Origin__NS": null,
            "IntegrationId__NS": null,
            "IntegrationStatus__NS": null,
            "Transaction__NS": null,
            "Test__c": null,
            "SyncDate__NS": null,
            "transferredToAccounting": "No",
            "excludeFromAutoApplyRules": false,
            "autoApplyUponPosting": false,
            "reversed": false,
            "taxStatus": "Complete",
            "sourceType": "Order",
            "taxMessage": null,
            "billToContactId": null,
            "billToContactSnapshotId": null,
            "sequenceSetId": null
        }
    ],
    "success": true
}
```


# Retrieve Object Information by API

Customers are always querying data for different purposes, and mainly for data analysis and daily operations. For example, they might want to retrieve the following object information:
- Accounts
- Orders
- Subscriptions
- Invoices
- Payments
- Credit Memos
- Debit Memos

In this case, we’ll show you how to retrieve the rate plans from Zuora and use the data in your portal.

The query call sends a query expression by specifying the object to query, the fields to retrieve from that object, and any filters to determine whether a given object should be queried.

In this guide, you will learn:
- How to fetch multiple data by API.

Let's use the retrievement of a specific account as the example.

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the GET Account API
You can simply input the specific account number into the request URL
```
curl --location --request GET 'https://rest.zuora.com/v1/accounts/{{accountNumber}}' \
--header 'Authorization: Bearer 0228427979714ca690ff07d4d648ac21'
```
## Step 3. Find the information from the response payload
Below is a sample response payload you may see
```
{
    "basicInfo": {
        "id": "",
        "name": "",
        "accountNumber": "",
        "notes": null,
        "status": "Active",
        "crmId": null,
        "batch": "Batch24",
        "invoiceTemplateId": "2c92c8fb79bde9d40179c075f2582ece",
        "communicationProfileId": null,
        "purchaseOrderNumber": null,
        "CustomerType__NS": "Company",
        "IntegrationStatus__NS": null,
        "Subsidiary__NS": null,
        "CollectionsAgent__c": null,
        "Location__NS": null,
        "Entity__c": null,
        "CustomerId__NS": null,
        "InCollections__c": "True",
        "SynctoNetSuite__NS": "Yes",
        "APM__c": "False",
        "WorkLocation__c": null,
        "IntegrationId__NS": null,
        "Department__NS": null,
        "RetryStatus__c": null,
        "Class__NS": null,
        "SyncDate__NS": null,
        "salesRep": null,
        "parentId": null,
        "creditMemoTemplateId": null,
        "debitMemoTemplateId": null,
        "sequenceSetId": null
    },
    "billingAndPayment": {
        "billCycleDay": 31,
        "currency": "USD",
        "paymentTerm": "Due Upon Receipt",
        "paymentGateway": "TestGateway",
        "defaultPaymentMethodId": "8a90876c84ae5ef70184c6c45fbc6a10",
        "invoiceDeliveryPrefsPrint": false,
        "invoiceDeliveryPrefsEmail": false,
        "additionalEmailAddresses": []
    },
    "metrics": {
        "balance": 0.000000000,
        "totalInvoiceBalance": 0.000000000,
        "creditBalance": 0.000000000,
        "totalDebitMemoBalance": 0.000000000,
        "unappliedPaymentAmount": 0.000000000,
        "unappliedCreditMemoAmount": 0.000000000,
        "contractedMrr": 4322.500000000
    },
    "billToContact": {
        "address1": "101 Redwood Shores Parkway",
        "address2": "",
        "city": "Redwood City",
        "country": "United States",
        "county": null,
        "fax": null,
        "firstName": "",
        "homePhone": null,
        "id": "8a90876c84ae5ef70184c6c45f9869ff",
        "lastName": "Sporer",
        "mobilePhone": null,
        "nickname": null,
        "otherPhone": null,
        "otherPhoneType": null,
        "personalEmail": null,
        "state": "California",
        "taxRegion": null,
        "workEmail": "",
        "workPhone": null,
        "zipCode": null,
        "contactDescription": null
    },
    "soldToContact": {
        "address1": "101 Redwood Shores Parkway",
        "address2": "",
        "city": "Redwood City",
        "country": "United States",
        "county": null,
        "fax": null,
        "firstName": "",
        "homePhone": null,
        "id": "8a90876c84ae5ef70184c6c45fa16a01",
        "lastName": "Sporer",
        "mobilePhone": null,
        "nickname": null,
        "otherPhone": null,
        "otherPhoneType": null,
        "personalEmail": null,
        "state": "California",
        "taxRegion": null,
        "workEmail": "",
        "workPhone": null,
        "zipCode": null,
        "contactDescription": null
    },
    "taxInfo": null,
    "success": true
}
```

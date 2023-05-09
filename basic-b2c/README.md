# Basic B2C Flow API Tutorial

In the business-to-consumer(B2C) market, consumer behavior is the primary driver. While some B2C businesses use their platforms to market and sell their own products, others make money by connecting buyers to sellers, using content traffic to sell advertisement spaces or restricting content to paid subscriptions.

By following this tutorial, you will learn how to build a classic B2C integration leveraging the Zuora REST API.

## Prerequisites

- You must have the following features enabled on your tenant:
  - **Orders**. If this feature is enabled in your tenant, you can see **Orders** under **Customers** in the left navigation panel in the Zuora UI. To obtain access to this feature, submit a request at <a href="https://support.zuora.com/" target="_blank">Zuora Global Support</a>.
  - **Invoice Settlement**. If this feature is enabled in your tenant, you can see **Credit and Debit Memos** under **Billing** in the left navigation panel in the Zuora UI. To obtain access to this feature, submit a request at <a href="https://support.zuora.com/" target="_blank">Zuora Global Support</a>.

- You must have an OAuth client created. See <a href="https://knowledgecenter.zuora.com/Billing/Tenant_Management/A_Administrator_Settings/Manage_Users#Create_an_OAuth_Client_for_a_User" target="_blank">Create an OAuth Client for a User</a> for more information.

## Get started with Zuora REST API

To get started and build an integration with Zuora through API, take the following steps: 

1. [Create an OAuth Token](1-authentication.md). You must create an OAuth token that will be used for subsequent API requests.
2. [Create products](2-create-product.md). This step creates the products or services to which your customers can subscribe.
3. [Sign up your customers](3-signup.md). This step explains how to complete the following tasks in one call: 
    - Create an account and an associated payment method
    - Create a subscription for this account
    - Generate an invoice
    - Collect payment
    
    If you want to complete only parts of the preceding tasks, see the following alternatives:
    - [Create accounts](3.1-create-account.md)
    - [Create payment methods](3.2-create-paymentmethod.md)
    - [Create subscriptions](3.3-create-subscription.md)
    - [Create billing documents](7-create-billing-document.md)
    - [Create payments](8-create-payments.md)

4. Optionally update subscriptions, such as:
    - [Update price on subscriptions](4-update-price.md)
    - [Cancel subscriptions and auto refund](5-cancel-subscription-with-refunds.md)
    - [Renew, suspend, or resume subscriptions](6-renew-suspend-resume-subscription)

For each step, you can use GET calls to retrieve information about the object you just created or updated. See [Retrieve Object Information by API](9-retrieve-object-info.md) for details.



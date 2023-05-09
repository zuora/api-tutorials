# Create Products
Products describe the goods or services you offer to your customers. Each product has a unique id and SKU.

Plans are collections of prices. While products help you track inventory or provisioning, plans and prices help you track payment terms.

Prices determine how you charge your customers for the product they subscribe to. Since products allow you to track inventory and provisioning you can change prices without having to change your provisioning scheme.

Prices can represent tiers, allowing the unit cost to change with quantity or usage. You might, for example, want to offer lower rates for customers who use more units per month. The following examples show two different ways to adjust pricing as usage increases: volume-based pricing and graduated pricing.

When you create a price with tiers, the unit cost varies depending on how many units your customer buys. 

In this guide, you will learn:
- How to create a product with tiered prices by volume through API.

## Step 1. Generate an OAuth token
See [Create an OAuth token](1-authentication.md) for details.

## Step 2. Submit a API request to the prodcut creation API

*API Sample payload with curl*
```
curl -X POST -H "Authorization: Bearer 6d151216ef504f65b8ff6e9e9e8356d3" -H "Content-Type: application/json" -d '{
    "Description": "Create product via API", 
    "EffectiveEndDate": "2066-10-20", 
    "EffectiveStartDate": "1966-10-20", 
    "Name": "P_1476935173677", 
    "SKU": "API-SKU1476935173677"
}' "https://rest.zuora.com/v1/object/product"
```

## Step 3. Verify the result
After the product is created, you can verify the result in the Zuora UI or through the GET API operations for the Prodcut object.

To verify the result through the Zuora UI, you can find the created product displayed at the top of the All Products page by navigating to **Prodcuts** > **Prodcut Catalog** in the Zuora UI.

To verify the result through the API, See [Retrieve Object Info](9-retrieve-object-info.md) for details.

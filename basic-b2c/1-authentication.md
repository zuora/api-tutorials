# Create an OAuth Token

You must create an OAuth token that will be used for API requests. After you have created an OAuth client with a client ID and client secret, use the [Create an OAuth token](https://www.zuora.com/developer/api-references/api/operation/createToken/) to create an OAuth token.

```
curl --location --request POST 'https://rest.zuora.com/oauth/token' \
        --header 'Content-Type: application/x-www-form-urlencoded' \
        --data-urlencode 'client_id={{your client ID}}' \
        --data-urlencode 'client_secret={{your client secret}}’ \
        --data-urlencode 'grant_type=client_credentials'
```
    
Alternative to using cURL, you can use the Zuora API collection in Postman to create an OAuth token:

<a href="https://www.postman.com/api-evangelist/workspace/zuora/overview" target="_blank">Download Postman</a>
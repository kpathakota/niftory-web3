# GraphQL & Auth Endpoints

The Niftory API is built in [**GraphQL**](https://graphql.org/)**.**&#x20;

{% swagger method="post" path=" " baseUrl="https://graphql.api.staging.niftory.com/" summary="" %}
{% swagger-description %}
Perform any GraphQL commands and actions. 
{% endswagger-description %}

{% swagger-parameter in="header" name="X-Niftory-API-Key" required="true" %}
Application's API Key
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
Bearer token containing the user's JWT. See 

[Authenticating the User](../core-concepts/authentication/configuring-your-app.md)

.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="SUCCESS" %}
```javascript
{
  "data": {
    "wallet": {
      "id": "14",
      "address": "0xf253fc2ca37c078436d07fb75e5a76a649892172",
      "state": "UNVERIFIED",
      "verificationCode": "xyz789",
      "nfts": [NFT],
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

### API Endpoints

{% hint style="warning" %}
**Always test your application against the Staging environment.** The production endpoint will affect the blockchain for real, so that should only be used when you are ready to ship your app.
{% endhint %}

#### Staging (targets blockchain testnet)

```url
https://graphql.api.staging.niftory.com
```

#### Production (targets blockchain prod)

```url
https://graphql.api.niftory.com
```

### Auth Service Endpoints

#### Production

```
NIFTORY_AUTH_SERVICE=https://auth.niftory.com
```

#### Staging

```
NIFTORY_AUTH_SERVICE=https://auth.staging.niftory.com
```

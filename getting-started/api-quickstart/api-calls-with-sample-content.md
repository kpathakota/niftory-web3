---
description: Explore the API against the sample app's content
---

# API calls with Sample Content

These API calls are against a common sample environment. To get your own application, follow the [Create Your Account](../your-niftory-account.md) steps after this!

### **Login to the Niftory Sample App at** [**sample.niftory.com**](https://sample.niftory.com)

This is a deployed version of our [Sample App](../../sample-app/niftory-sample-app/), managed by Niftory.

### Copy the API Key and Authorization header from the app

For your convenience, the app's API key is printed out for you, and authorization header can be copied after you log in to the app:

![](<../../.gitbook/assets/image (3).png>)

### **Go to our** [**GraphQL Playground**](https://studio.apollographql.com/public/Niftory-Staging-1wxews/home?variant=current) **or** [**Postman Collection**](https://www.postman.com/dark-star-402968/workspace/niftory/collection/22409520-3f7f5cea-fe22-4589-beb5-5ff8d6fdca12?ctx=documentation)

Add the values you copied from the previous sections to these request headers:

* `X-Niftory-API-Key`
* `Authorization`

{% hint style="info" %}
In Apollo Studio, copy/paste the values into the environment variables section as "`apiKey"` and "`token`"
{% endhint %}

{% embed url="https://studio.apollographql.com/public/Niftory-Staging-1wxews/home?variant=current" %}
**Niftory GraphQL Playground**
{% endembed %}

### **Try making API calls**

![Niftory GraphQL Playground](<../../.gitbook/assets/image (1).png>)

Here's an example query to try:

```graphql
query NFTModelsQuery {
  nftModels {
    items {
      id
      title
      description     
    }
  }
}
```

This query returns the list of NFTModels that the Sample App has available for users to mint an NFT from. For more queries, see [Querying NFTs](../../core-concepts/nfts/querying-nfts.md).

**Learn more about the Niftory API**:

{% content-ref url="../../core-concepts/niftory-data-model.md" %}
[niftory-data-model.md](../../core-concepts/niftory-data-model.md)
{% endcontent-ref %}

{% content-ref url="../api-cheat-sheet.md" %}
[api-cheat-sheet.md](../api-cheat-sheet.md)
{% endcontent-ref %}

{% hint style="success" %}
Now you're ready to build your own - we recommend you start with the [Niftory Sample App](../../sample-app/niftory-sample-app/) to get the basics set up!
{% endhint %}

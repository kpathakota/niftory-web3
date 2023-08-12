# App and AppUser

{% hint style="info" %}
If your app handles user authentication already, you can skip the AppUser data model, and use Niftory directly with wallets. All APIs work with both a wallet address/ID and a user ID. The main difference is if you are bypassing AppUser, then all APIs must be called with [Privileged Authentication](authentication/privileged-authentication.md) (i.e. by passing in both your API Key and Client Secret).
{% endhint %}

Check out [Anatomy of a Niftory App](../sample-app/anatomy-of-a-niftory-app/#appuser) to see how these constructs are used in practice.&#x20;

### [App](https://api-docs-niftory.vercel.app/#definition-App)

Represents your application in the Niftory ecosystem.

{% tabs %}
{% tab title="GraphQL" %}
[app](https://api-docs-niftory.vercel.app/#query-app)

```graphql
query AppQuery {
  app { 
    id 
  }
}
```


{% endtab %}
{% endtabs %}

<details>

<summary>Sample Response</summary>

```json
{
  "data": {
    "app": {
      "id": 56
    }
  }
}
```

</details>

### [AppUser](https://api-docs-niftory.vercel.app/#definition-AppUser)

Representation of a user logged in to your application via Niftory's [authentication service](authentication/configuring-your-app.md).

{% tabs %}
{% tab title="GraphQL" %}
[appUser](https://api-docs-niftory.vercel.app/#query-appUser)

```graphql
query AppUserQuery {
  appUser {
    email
    image
    name
    id
    wallet {
      ...WalletFragment
    }
    app {
      ...AppFragment
    }
  }
}
```
{% endtab %}
{% endtabs %}

<details>

<summary>Sample Response</summary>

```json
{
  "data": {
    "appUser": {
      "email": "user@test.com",
      "image": "linkToImage",
      "name": "Nif Tory",
      "id": 3,
      "wallet": Wallet,
      "app": App
    }
  }
}
```

</details>

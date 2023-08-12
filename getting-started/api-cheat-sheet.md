# API Cheat Sheet

The Niftory API is built in [**GraphQL**](https://graphql.org/)**.** We also plan to support [REST](http://en.wikipedia.org/wiki/Representational\_State\_Transfer) endpoints in the future. Our GraphQL Endpoint can be found here:

{% content-ref url="../reference/graphql-and-auth-endpoints.md" %}
[graphql-and-auth-endpoints.md](../reference/graphql-and-auth-endpoints.md)
{% endcontent-ref %}

### **Why start with GraphQL?**

GraphQL has become a well-established standard for [building performant web applications](https://graphql.org/faq/#why-should-i-use-graphql), giving the power of what to query and how much to query to the app developer (with strong typing!). This is consistent with [our philosophy](../#our-philosophy) of making it as easy as possible for you to quickly launch high-performing experiences.

> If you have a strong need for REST endpoints for your application, please [**reach out to us**](../reference/support-and-feedback.md)**.**&#x20;

### Niftory Data Model

Here are the core concepts in the Niftory data model.

![For more details, see the Data Model guide](../.gitbook/assets/NiftoryDataModel2.png)

### API Endpoints

{% hint style="warning" %}
**Always test your application against the Staging environment.** The production endpoint will affect the blockchain for real, so that should only be used when you are ready to ship your app.
{% endhint %}

```url
https://graphql.api.staging.niftory.com
```

#### Production (targets blockchain prod)

```url
https://graphql.api.niftory.com
```

### Authentication

{% hint style="success" %}
Remember to [**get your API keys**](../core-concepts/authentication/using-your-api-key.md) to set up your application!
{% endhint %}

More details in the Authentication guide:

{% content-ref url="../core-concepts/authentication/" %}
[authentication](../core-concepts/authentication/)
{% endcontent-ref %}

### APIs at a Glance

{% hint style="info" %}
[See Niftory API Reference **here**](https://graphql.docs.niftory.com) :bulb:
{% endhint %}

<details>

<summary><a href="../core-concepts/app-and-appuser.md#app">App</a></summary>

* Representation of your application in Niftory ecosystem.
* **Queries:**&#x20;
  * [app](https://graphql.docs.niftory.com/#query-app) - returns the current App context.

</details>

<details>

<summary><a href="../core-concepts/app-and-appuser.md#appuser">AppUser</a></summary>

* Representation of a user logged in to your application via Niftory's [authentication service](../core-concepts/authentication/configuring-your-app.md).
* **Queries:**&#x20;
  * [appUser](https://api-docs-niftory.vercel.app/#query-appUser) - returns the currently logged-in AppUser context.



</details>

<details>

<summary><a href="../core-concepts/wallets/register-external-wallets.md">Wallet</a></summary>

* Representation of a user's blockchain wallet scoped to your application. For more information, see [Wallet Setup](../core-concepts/wallets/register-external-wallets.md).

<!---->

* [**Queries**](../core-concepts/wallets/register-external-wallets.md#wallet-queries)**:**
  * [wallet](https://api-docs-niftory.vercel.app/#query-wallet) - returns the currently logged-in user's Wallet.

<!---->

* [**Mutations**](../core-concepts/wallets/register-external-wallets.md#wallet-creation-and-verification)**:**
  * [createNiftoryWallet](https://graphql.docs.niftory.com/#mutation-createNiftoryWallet) - creates a custodial Niftory wallet
  * [registerWallet](https://api-docs-niftory.vercel.app/#mutation-registerWallet) - registers the given wallet address to the currently logged-in user.
  * [verifyWallet](https://api-docs-niftory.vercel.app/#mutation-verifyWallet) - verifies that the user actually owns the Wallet by asking them to sign a secret.
  * [readyWallet](https://api-docs-niftory.vercel.app/#mutation-readyWallet) - marks a Wallet as ready for usage for the application.



</details>

{% hint style="info" %}
Your application needs to **registerWallet --> verifyWallet --> readyWallet** for any given user in order to set up the wallet to receive NFTs. [Read more here](../core-concepts/wallets/register-external-wallets.md).
{% endhint %}

<details>

<summary><a href="../core-concepts/nfts/#niftory-nft-data-model">NFT</a></summary>

* A representation of an [NFT](https://en.wikipedia.org/wiki/Non-fungible\_token) (it doesn't have to be minted yet).
* **Queries:**
  * [nfts](https://api-docs-niftory.vercel.app/#query-nfts) - returns all NFTs belonging to the currently logged-in AppUser.
  * [nft](https://api-docs-niftory.vercel.app/#query-nft) - returns an NFT by its Niftory database ID.

<!---->

* **Mutations:**
  * [transfer](https://api-docs-niftory.vercel.app/#mutation-transfer) - initiates the transfer of a specified NFT to a specified user. **This API may mint the NFT if it isn't yet minted.**

</details>

<details>

<summary><a href="../core-concepts/nfts/#niftory-nft-data-model">NFTModel</a></summary>

A blueprint for an NFT, containing everything needed to mint one -- file content, blockchain metadata, etc.

* **Queries:**
  * [nftModels](https://api-docs-niftory.vercel.app/#query-nftModels) - returns the NFTModels belonging to the current App context.
  * [nftModel](https://api-docs-niftory.vercel.app/#query-nftModel) - returns an NFTModel by its Niftory database ID.\

* **Mutations:**
  * [createNFTModel](https://graphql.docs.niftory.com/#mutation-createNFTModel) / [updateNFTModel](https://graphql.docs.niftory.com/#mutation-updateNFTModel) - creates/updates an NFTModel

</details>

<details>

<summary><a href="../core-concepts/nfts/#niftory-nft-data-model">NFTSet</a></summary>

* A bag of NFTModels, to help you organize your NFTs
* **Queries:**
  * [sets](https://api-docs-niftory.vercel.app/#query-sets) - returns the NFTSets belonging to the current App context.
  * [set](https://api-docs-niftory.vercel.app/#query-set) - returns an NFTSet by its Niftory database ID.
* **Mutations:**
  * [createNFTSet](https://graphql.docs.niftory.com/#mutation-createNFTSet) / [updateNFTSet](https://graphql.docs.niftory.com/#mutation-updateNFTSet) - creates/updates an NFTSet

</details>

{% hint style="info" %}
An **NFTSet** is a bag of NFTModels

An **NFTModel** is a bag of`NFTs`

An **NFT** is the thing that a user actually owns.
{% endhint %}

{% hint style="info" %}
You can also use the Admin Portal to [**create and manage**](https://docs.niftory.com/docs/v/niftory-admin/fundamentals/nft-collection) **NFTSets** and **NFTModels** for your application.
{% endhint %}

Happy Coding!

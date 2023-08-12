# API Usage

Now we've set up the GraphQL client and authentication, let's explore the app and its use of the Niftory API surface to build these core capabilities:

* **User Profile**: Get the Profile of the user (name, email, image, and etc). Performed via the [AppUser queries](api-usage.md#appuser). A deep dive on these queries is available in [App and AppUser](../../core-concepts/app-and-appuser.md).
* **NFTs**: Get all of NFTs that are available through your application, and all of the NFTs that belong to a user using the [**NFTModels**](api-usage.md#querying-available-nfts) and [**NFTs**](api-usage.md#nfts) queries respectively.
* **Wallets:** [**Setup a wallet**](api-usage.md#wallet-setup) for a new user, or query the user's wallet via the [**Wallets**](api-usage.md#wallets) query.&#x20;

### AppUser

With your GraphQL client set up, every request you make to the Niftory API service will automatically contain the logged-in user's authentication token, and your app's API key. This allows the API service to implicitly infer the context of the caller.

For example, to get the `AppUser` object representing the currently logged-in user, you simply have to run the [`appUser`](https://api-docs-niftory.vercel.app/#query-appUser) query, without any arguments!

```graphql
const GET_APP_USER = gql`
  query {
    appUser {
      id
      name
      email
      image
      app {
        id
      }
      wallet {
        id
        address
        state
      }
    }
  }
`;
```

{% hint style="success" %}
We recommend wrapping your GraphQL queries in [React Hooks](https://reactjs.org/docs/hooks-intro.html). For example, the above query can be part of a [`useUser`](https://github.com/Niftory/niftory-samples/blob/ac646597ee797fad49e0468b6faa1c5045c515f9/basic-app/hooks/useUser.tsx) hook, so you can easily get the user context anywhere in your application.
{% endhint %}

### NFTs

Now for the fun part! Let's get some NFTs to display and transfer:

{% hint style="info" %}
Note that to actually _**create**_ NFT content, use the [Niftory Admin Portal](../../getting-started/niftory-admin-portal.md) -- the same place you got your API keys from. You can create your NFT collection there.
{% endhint %}

#### Querying Available NFTs

In order to display the NFT content available through your app, use the [`nftModels`](https://api-docs-niftory.vercel.app/#query-nftModels) API. This query returns all the [`NFTModel`](https://api-docs-niftory.vercel.app/#definition-NFTModel)`'s` that you have marked as Available in the Admin Portal.

Recall that an NFTModel is basically a blueprint for an NFT -- it contains all the content and metadata that would get minted. You can limit the supply of NFTs for an NFTModel by setting its `quantity` appropriately (using the Admin Portal [as follows](https://docs.niftory.com/docs/v/niftory-admin/fundamentals/nft-collection/collectibles)).

```graphql
const GET_NFT_MODELS = gql`
  query {
    nftModels {
      id
      blockchainId
      title
      description
      quantity
      status
      rarity
      content {
        files {
          media {
            url
            contentType
          }
          thumbnail {
            url
            contentType
          }
        }
        poster {
          url
        }
      }
    }
  }
`;
```

While the query works without any arguments, you can optionally specify a [`filter`](https://api-docs-niftory.vercel.app/#definition-NFTModelFilterInput) (e.g. get only NFTs belonging to a particular [`Set`](https://api-docs-niftory.vercel.app/#definition-NFTSet))

#### Querying User's NFT Collection

Use the [`nfts`](https://api-docs-niftory.vercel.app/#query-nfts) query to get `NFT's` belonging to the currently logged-in user. Like the `nftModels` query, you can also [filter](https://api-docs-niftory.vercel.app/#definition-NFTFilterInput) this query as needed (by particular `NFTModel`'s, for example).

```graphql
const GET_USER_NFTs = gql`
  query {
    nfts {
      id
      model {
        id
        title
      }
    }
  }
`;
```

The sample app wraps all these queries in React Hooks; we'd recommend you to do the same to easily use these objects throughout your application.

{% hint style="info" %}
The queries above don't query the blockchain directly. Instead, they query Niftory's database representation of the blockchain, which helps make them very fast, and allows non-blockchain data (such as not-yet-minted NFTModels) to be retrieved from the same API.
{% endhint %}

### Wallets

#### Querying a Wallet

To get the currently logged-in user's `Wallet` simply call the [`wallet`](https://api-docs-niftory.vercel.app/#query-wallet) query without any arguments:

```graphql
const GET_USER_WALLET = gql`
  query {
    wallet {
      id
      address
      state
      verificationCode
    }
  }
`;
```

#### Wallet Setup

The [Wallet Setup](../../core-concepts/wallets/register-external-wallets.md) guide explains in detail how to setup a user's wallet, but the high level idea is this:

* Call [`registerWallet`](https://api-docs-niftory.vercel.app/#mutation-registerWallet) mutation to associate a Wallet address with an `AppUser.`

```graphql
const REGISTER_WALLET = gql`
  mutation ($address: String!) {
    registerWallet(address: $address) {
      id
      address
      verificationCode
      state
    }
  }
`;
```

* Call [`verifyWallet`](https://api-docs-niftory.vercel.app/#mutation-verifyWallet) with a signed verification code (the verification code is returned by the `registerWallet` mutation).

```graphql
const VERIFY_WALLET = gql`
  mutation ($address: String!, $signedVerificationCode: JSON!) {
    verifyWallet(
      address: $address
      signedVerificationCode: $signedVerificationCode
    ) {
      id
      address
      state
    }
  }
`;
```

* Finally, call [`readyWallet`](https://api-docs-niftory.vercel.app/#mutation-readyWallet) to mark the Wallet as initialized for your application.

```graphql
const READY_WALLET = gql`
  mutation ($address: String!) {
    readyWallet(address: $address) {
      id
      address
      state
    }
  }
`;
```

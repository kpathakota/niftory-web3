# Contract

[`Contract`](https://api-docs-niftory.vercel.app/#definition-Contract) represents a [smart contract](https://en.wikipedia.org/wiki/Smart\_contract) on the blockchain.

### Contract Deployment

When you sign up in the [Niftory Admin Portal](../getting-started/niftory-admin-portal.md), we help you automatically deploy a contract for your application. All NFTs minted for your application are based on this contract, and user wallets have to be [initialized for this contract](wallets/register-external-wallets.md#3.-mark-the-wallet-as-ready-for-your-app).

For details on how to deploy a smart contract, follow this [guide](../sample-app/niftory-sample-app/).

### Contract API

There are several ways to query for your Contract.

#### [contract](https://api-docs-niftory.vercel.app/#query-contract) - returns the [Contract](https://api-docs-niftory.vercel.app/#definition-Contract) for the current [App](https://api-docs-niftory.vercel.app/#definition-App) context (determined by API Key)

```graphql
query ContractQuery {
  contract {
    id
    address
    blockchain
    name
  }
}
```

<details>

<summary>Sample Response</summary>

```
{
  "data": {
    "contract": {
      "id": "56",
      "address": "0xf8d6e0586b0a20c7",
      "blockchain": "FLOW",
      "name": "MyApp Contract",
    }
  }
}
```

</details>

#### [app](https://api-docs-niftory.vercel.app/#query-app) query (and include the Contract fragment)&#x20;

```graphql
query AppWithContractQuery {
  app {
    id
    contract
  }
}
```

# Query Wallets

A [Wallet](https://api-docs-niftory.vercel.app/#definition-Wallet) is the representation of a user's blockchain wallet scoped to your application. There are several ways to query for a wallet. The simplest way to get a logged-in user's wallet is with the `wallet` and `appUser` queries:

#### [wallet](https://api-docs-niftory.vercel.app/#query-wallet) - returns the currently logged-in user's [Wallet](https://api-docs-niftory.vercel.app/#definition-Wallet).n

```graphql
query WalletQuery {
  wallet {
    id
    address
    state
    verificationCode
    nfts {
      ...NFTFragment
    }
  }
}
```

<details>

<summary>Sample Response</summary>

```
{
  "data": {
    "wallet": {
      "id": "14",
      "address": "0xf253fc2cb42c078436d07fb77e5a76a649892172",
      "state": "UNVERIFIED",
      "verificationCode": "xyz789",
      "nfts": ["NFT"],
    }
  }
}
```

</details>

#### [appUser](../app-and-appuser.md#appuser) query (and include the Wallet fragment)&#x20;

```graphql
query AppUserWithWalletQuery {
  appUser {
    email
    wallet {
      ...WalletFragment
    }
  }
}
```

The following queries are privileged APIs that can only be [invoked from your application backend](broken-reference):

* [walletById](https://api-docs-niftory.vercel.app/#query-walletById) - returns a Wallet by its Niftory database ID.
* [walletByAddress](https://api-docs-niftory.vercel.app/#query-walletByAddress) - returns a Wallet by its Niftory address.
* [walletByUserId](https://api-docs-niftory.vercel.app/#query-walletByUserId) - returns a Wallet for a given AppUser ID.

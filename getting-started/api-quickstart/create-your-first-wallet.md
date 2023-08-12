# Create your first Wallet

{% hint style="info" %}
Go through the [Niftory Quickstart](../niftory-quickstart.md) before starting this section.
{% endhint %}

Niftory provides a simple API to create a custodial wallet. You don't even need to specify any args!

If you've set up the API Playground from [Quickstart](./), try executing the following mutation:

```graphql
mutation CreateWallet {
    createNiftoryWallet {
        id
        address
        state
    }
}
```

It's that simple to create a new wallet. The `state` should tell you if the wallet is ready for use yet. You can query the wallet using the `walletById` query:

```graphql
query WalletById($id: ID!) {
  walletById(id: $id) {
    id
    address
    state
  }
}
```

And pass in the `id` args:

```json
{
  "id": "id-returned-by-createWallet-mutation"
}
```

{% hint style="success" %}
This Wallet is automatically configured to receive NFTs minted with your [deployed contract](./#deploy-your-smart-contract).
{% endhint %}

To learn more about Niftory's Wallet APIs (including registering and verifying external wallets), visit the Wallet guide:

{% content-ref url="../../core-concepts/wallets/" %}
[wallets](../../core-concepts/wallets/)
{% endcontent-ref %}

# Mint your first NFT

{% hint style="info" %}
Go through [API Quickstart](./) before starting this section.
{% endhint %}

There are a couple of ways you can design and mint an NFT.

## Option 1 - Design in Admin Portal, mint using API

### Design your NFT

1. Log into the [Admin Portal](https://admin.niftory.com), and open Collectibles
2. Create a Set
3. Create a Collectible inside the Set.&#x20;
   * Specify the total supply of the Collectible (i.e. how many NFTs of this shape can be created) as `quantity`
   * Upload media content, which automatically gets uploaded to the InterPlanetary File System (IPFS).
   * Add any custom metadata you would like to be part of your NFT.

{% hint style="info" %}
Follow [**this guide**](https://docs.niftory.com/home/v/admin/guides/create-your-first-nft) for more information on navigating the Admin Portal.
{% endhint %}

### Query

Let's query what we just created. At this point it's just a blueprint of what an NFT would look like when minted; it doesn't exist on the blockchain yet. This "NFT blueprint" is called an `NFTModel` in the API.

The following query returns a paginated list of all the NFTModels you've created for your app so far. Since we just created one, we should get a single item back.

```graphql
query NFTModels($appId: ID) {
  nftModels(appId: $appId, maxResults: 1) {
    items {
      id
      title
      quantity
      status
      set {
        id
      }
      content {
        files {
          url
          contentType
        }
        poster {
          url
        }
      }
    }
    cursor
  }
}
```

For `appId`, pass in your app's client ID (available from the Your App page in Admin Portal).&#x20;

You can find other ways of querying for NFTs belonging to an app, a user or a wallet in the following guide:

{% content-ref url="../../core-concepts/nfts/querying-nfts.md" %}
[querying-nfts.md](../../core-concepts/nfts/querying-nfts.md)
{% endcontent-ref %}

### Mint

It's time to mint! Take the `id` of the `NFTModel`returned by the query above, and execute the following mutation:

```graphql
mutation Mint(
  $id: ID!,
  $quantity: PositiveInt
) {
  mintNFTModel(id: $id, quantity: $quantity) {
    id
    blockchainId
    metadata
    state
  }
}
```

This kicks off minting asynchronously, which can be tracked by checking the value of `state` on `NFTModel`.

Now you can query `NFTModel` to see when minting is completed.

```graphql
query NFTModel($id: ID!) {
  nftModel(id: $id) {
    id
    state
    nfts {
      id
      blockchainState
      blockchainId
      serialNumber
      metadata
    }
  }
}
```

Congratulations on making it this far! Don't worry if some of this still doesn't make sense. The next sections will go over the API in more detail. You already have enough context to start adding NFTs to your application.

## Option 2 - Create everything with API

This section details how to create an NFT entirely using the API - uploading files, setting metadata, and minting:

{% content-ref url="../../core-concepts/nfts/creating-nfts.md" %}
[creating-nfts.md](../../core-concepts/nfts/creating-nfts.md)
{% endcontent-ref %}

## Bonus - Transfer NFT to your Wallet

If you created a wallet in the [previous guide](create-your-first-wallet.md), you can use the `transfer` API to mint and transfer an NFT to your wallet in one go!

```graphql
mutation Transfer($address: String, $nftModelId: ID) {
  transfer(address: $address, nftModelId: $nftModelId) {
    id
    blockchainState
    saleState
    serialNumber
    blockchainId
    model {
      id
      title
    }
  }
}
```

Pass in the `address` of your Wallet, and the ID of the `NFTModel` from the previous section.

{% hint style="info" %}
This API showcases the benefits of the web2 + web3 approach of the Niftory API: while minting and transfer can be time-consuming operations on the blockchain, you get an `NFT` object back instantly, whose state gets updated asynchronously. This allows you as an app developer to build very responsive web3 apps with eventual consistency with the blockchain.
{% endhint %}

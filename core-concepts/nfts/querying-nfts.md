# Querying NFTs

## Querying Available NFT Models

In order to display the NFT content available for your app, use the [`nftModels`](https://api-docs-niftory.vercel.app/#query-nftModels) API. This query returns all the [`NFTModel`](https://api-docs-niftory.vercel.app/#definition-NFTModel) that you have marked as Available in the Admin Portal&#x20;

{% hint style="info" %}
In Admin Portal, NFTModel is referred to as Collectible.
{% endhint %}

Recall that an NFTModel is basically a blueprint for an NFT -- it contains all the content and metadata that would get minted into an NFT. You can limit the supply of NFTs for an NFTModel by setting its `quantity` appropriately (using the Admin Portal [as follows](https://docs.niftory.com/docs/v/niftory-admin/fundamentals/nft-collection/collectibles)).

#### [nftModels](https://api-docs-niftory.vercel.app/#query-nftModels) - returns [NFTModel](https://api-docs-niftory.vercel.app/#definition-NFTModel)'s for the current [App](https://api-docs-niftory.vercel.app/#definition-App) context.

```graphql
query NFTModelsQuery($filter: NFTModelFilterInput) {
  nftModels(filter: $filter) {
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
```

**Note:** You can do some rich filtering on which NFTModels to display in your application using the (optional) [NFTModelFilterInput](https://api-docs-niftory.vercel.app/#definition-NFTModelFilterInput) parameter.

<details>

<summary>Query NFTModels Sample Response</summary>

```
{
  "data": {
    "nftModels": [{
      "status": "AVAILABLE",
      "blockchainId": "f253fc2cb42c078436d07fb77e5a76a649892172",
      "id": "45",
      "title": "First NFT Drop",
      "description": "MyApp's First NFT",
      "quantity": 123,
      "content": NFTContent,
      "set": NFTSet,
      "nfts": [NFT]
    }]
  }
}
```

</details>

<details>

<summary>Query an NFTModel by ID</summary>

#### [nftModel](https://api-docs-niftory.vercel.app/#query-nftModel) - returns an [NFTModel](https://api-docs-niftory.vercel.app/#definition-NFTModel) by its Niftory database ID.

```graphql
query NftModelQuery($id: String!) {
  nftModel(id: $id) {
    blockchainId
    metadata
    id
    title
    description
    rarity
    quantity
    content {
      ...NFTContentFragment
    }
    set {
      ...NFTSetFragment
    }
    nfts {
      ...NFTFragment
    }
  }
}
```

</details>

## Querying User NFTs

Use the [`nfts`](https://api-docs-niftory.vercel.app/#query-nfts) query to get `NFT's` belonging to the currently logged-in user. Like the `nftModels` query, you can optionally [filter](https://api-docs-niftory.vercel.app/#definition-NFTFilterInput) this query as needed (by particular `NFTModel`'s, for example).

[nfts](https://api-docs-niftory.vercel.app/#query-nfts) - Gets all [NFT](https://api-docs-niftory.vercel.app/#definition-NFT)'s belonging to the current [AppUser](https://api-docs-niftory.vercel.app/#definition-AppUser) context.

```graphql
query NFTsQuery($filter: NFTFilterInput) {
  nfts(filter: $filter) {
    id
    blockchainId
    serialNumber
    metadata
    model {
      id
    }
    wallet {
      address
    }
    status
  }
}
```

{% hint style="info" %}
The queries above don't query the blockchain directly. Instead, they query Niftory's database representation of the blockchain, which helps make them very fast, and allows non-blockchain data (such as not-yet-minted NFTModels) to be retrieved from the same API.
{% endhint %}

{% hint style="info" %}
**NFTSet, NFTModel** and **NFT** types all have `blockchainId`, which is set once an NFT is minted, and can be used to look up these entities on the blockchain itself.
{% endhint %}

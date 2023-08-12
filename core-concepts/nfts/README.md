# NFTs

{% hint style="info" %}
Check out [Anatomy of a Niftory App](../../sample-app/anatomy-of-a-niftory-app/#wallets) to see how these constructs are used in practice.
{% endhint %}

### Niftory NFT Data Model

There are 3 key concepts that yield an [NFT](https://en.wikipedia.org/wiki/Non-fungible\_token) in the Niftory ecosystem:

* [**NFTSet**](https://api-docs-niftory.vercel.app/#definition-NFTSet) - A bag of NFTModels, to help you organize your NFTs
* [**NFTModel**](https://api-docs-niftory.vercel.app/#definition-NFTModel) - A blueprint for an NFT, containing everything needed to mint one -- file content, blockchain metadata, etc.
* [**NFT**](https://api-docs-niftory.vercel.app/#definition-NFT) - A representation of an [NFT](https://en.wikipedia.org/wiki/Non-fungible\_token) (it doesn't have to be minted yet).

{% hint style="info" %}
An **NFTSet** contains a number of **NFTModels**, which contain a number of **NFTs**
{% endhint %}

### What gets put on the blockchain, and when?

`NFT`, `NFTModel` and `NFTSet` are all blockchain concepts. i.e. when you mint, the [Niftory contract](https://github.com/Niftory/niftory-beam-cadence/blob/main/cadence/contracts/Beam.cdc) creates a [Set](https://github.com/Niftory/niftory-beam-cadence/blob/main/cadence/contracts/Beam.cdc#L227) for an NFTSet, a [CollectibleItem](https://github.com/Niftory/niftory-beam-cadence/blob/main/cadence/contracts/Beam.cdc#L138) for an NFTModel, and a [NonFungibleToken](https://github.com/Niftory/niftory-beam-cadence/blob/main/cadence/contracts/Beam.cdc#L545) for an NFT on the blockchain.

Note that because Niftory supports minting lazily, you can create all these objects in the Admin Portal without minting them, display them to users, and delay the mint step until the actual [NFT Transfer](transferring-nfts.md) (which will handle minting automatically).

This is possible because Niftory API combines both on-chain and off-chain data into one API. When you are querying the API, it is returning data from the Niftory database. The database is updated automatically to represent the source-of-truth on the blockchain.

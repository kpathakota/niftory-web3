# Transferring NFTs

The [`transfer`](https://api-docs-niftory.vercel.app/#mutation-transfer) mutation is a privileged API that allows the application backend to transfer an NFT to a user. This API is designed to be securely used from a backend so you'll need to use a privileged token in order to use it.&#x20;

{% hint style="info" %}
For more details on invoking privileged APIs, see [Niftory APIs in your backend](broken-reference).
{% endhint %}

The `transfer` API kicks off the following operations:

* Takes in the ID of the [NFT](https://api-docs-niftory.vercel.app/#definition-NFT) or [NFTModel](https://api-docs-niftory.vercel.app/#definition-NFTModel) to transfer. If an NFTModel is specified, the API selects an NFT from the NFTModel.
* Mints the NFT if it hasn't already been minted, and
* Transfers the minted NFT to the user's wallet

Since these can be time-consuming operations, the `transfer` API schedules the work to happen asynchronously, and returns back the `NFT` that _will be_ transferred to the user. The `NFT`'s [`status`](https://api-docs-niftory.vercel.app/#definition-NFT) field tells you what is going on.

```graphql
mutation transferNFTToUser($nftModelId: ID!, $userId: ID!) {
  transfer(nftModelId: $nftModelId, userId: $userId) {
    id
    status
  }
}
```

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/pages/api/nft/[nftModelId]/transfer.ts">Transfer handler in application backend</a></summary>

```javascript
const handler: NextApiHandler = async (req, res) => {
  const { nftModelId, userId } = req.query;

  if (req.method !== "POST") {
    res.status(405).end();
    return;
  }

  const signedIn = !!getToken({ req });
  if (!signedIn) {
    res.status(401).send("You must be signed in to transfer NFTs");
  }

  if (!nftModelId) {
    res.status(400).send("nftModelId is required");
    return;
  }

  const client = await getBackendGraphQLClient();
  const sdk = getSdk(client);

  const data = await sdk.transferNFTToUser({
    nftModelId: nftModelId as string,
    userId: userId as string,
  });

  res.status(200).json(data);
};
```

</details>

Follow the [sample app's structure](https://github.com/Niftory/niftory-samples/blob/main/basic-app/pages/api/nft/\[nftModelId]/transfer.ts) for an example on how to invoke the `transfer` mutation.

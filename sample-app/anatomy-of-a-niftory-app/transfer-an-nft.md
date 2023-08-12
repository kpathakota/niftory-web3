# Transfer an NFT

The final core capability of the sample app is the ability to transfer NFTs to users. This is accomplished via the [`transfer`](https://api-docs-niftory.vercel.app/#mutation-transfer) mutation, which:

* Takes in the ID of the NFT or NFTModel to transfer
* Mints the NFT if it hasn't already been minted, and
* Transfers the minted NFT to the user's wallet

Since these can be time-consuming operations, the `transfer` API schedules the work to happen asynchronously, and returns back the `NFT` that _will be_ transferred to the user. The `NFT`'s [`status`](https://api-docs-niftory.vercel.app/#definition-NFT) field tells you what is going on.

{% hint style="info" %}
Unlike the other APIs we have seen so far that are run in the context of the logged-in user, **the `transfer`API is** [**privileged**](broken-reference). As a result, you will need to call `transfer` from your application's backend.
{% endhint %}

```graphql
const TRANSFER = gql`
  mutation transferNFTToUser($nftModelId: ID!, $userId: ID!) {
    transfer(nftModelId: $nftModelId, userId: $userId) {
      id
    }
  }
`;
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

Follow the sample app's structure for an example on how to invoke the `transfer` mutation, or read further on how to invoke[ Niftory APIs in your backend](broken-reference).

You now have everything you need to get started—except the NFTs themselves! So let's discuss the Niftory Admin Portal next, where you can set up your NFT collections and manage your app.

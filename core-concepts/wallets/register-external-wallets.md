# Register External Wallets

{% hint style="info" %}
Check out [Anatomy of a Niftory App](../../sample-app/anatomy-of-a-niftory-app/#wallets) to see how these constructs are used in practice.
{% endhint %}

A [Wallet](https://api-docs-niftory.vercel.app/#definition-Wallet) is the representation of a user's blockchain wallet scoped to your application.

### Wallet Registration and Verification

Wallet setup is a 3-step process. This is done to ensure trust and security at every stage.

We understand that this process is toilsome, and are working on ways to handle it for you during the authentication flow.&#x20;

#### **1.** Register the Wallet with Niftory.

[registerWallet](https://api-docs-niftory.vercel.app/#mutation-registerWallet) registers the given wallet address to the currently logged-in user.

You should prompt your user to sign into their wallet, and then call this API with the address you get back. Check out [Flow's documentation](https://docs.onflow.org/fcl/reference/authentication/) for how to do this on the Flow blockchain.

{% hint style="info" %}
`registerWallet` is required before any of the Wallet [queries](register-external-wallets.md#wallet-queries) start returning any data. This is because Niftory APIs don't query the blockchain directly, and need the user-wallet association explicitly defined.
{% endhint %}

```graphql
mutation RegisterWalletMutation($address: String!) {
  registerWallet(address: $address) {
    id
    address
    state
    verificationCode
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
      "state": "UNVERIFIED"
    }
  }
}
```

</details>

#### 2. Verify the Wallet belongs to the AppUser

[verifyWallet](https://api-docs-niftory.vercel.app/#mutation-verifyWallet) verifies that the user actually owns the wallet.

The verification code to sign is part of the Wallet object, and can be returned by the `registerWallet` mutation.

Once you have the verification code, you should prompt your user to sign it. On Flow, this is done by calling [`fcl.currentUser.signUserMessage`](https://docs.onflow.org/fcl/reference/user-signatures/#currentusersignusermessage).  Once you receive the signature back, include it as the signedVerificationCode parameter to the verifyWallet API:

```graphql
mutation VerifyWalletMutation(
  $address: String!,
  $signedVerificationCode: JSON!
) {
  verifyWallet(
    address: $address,
    signedVerificationCode: $signedVerificationCode
  ) {
    id
    address
    state
  }
}
```

<details>

<summary>Sample Response</summary>

```json
{
  "data": {
    "wallet": {
      "id": "14",
      "address": "0xf253fc2cb42c078436d07fb77e5a76a649892172",
      "state": "VERIFIED"
    }
  }
}
```

</details>

#### 3. Mark the Wallet as ready for your App

[readyWallet](https://api-docs-niftory.vercel.app/#mutation-readyWallet) - marks a Wallet as ready to receive NFTs. The wallet must be verified.

On the flow blockchain, wallets usually need to run a transaction before being able to receive NFTs, and this transaction must be signed by the owner of the wallet. Therefore, your app needs to prompt the user to authorize this transaction and let us know this has been done by calling the `readyWallet` API.

See the sample app for [an example on how to do this](https://github.com/Niftory/niftory-samples/blob/main/basic-app/hooks/useFlowAccountConfiguration.tsx).

```graphql
mutation ReadyWalletMutation($address: String!) {
  readyWallet(address: $address) {
    id
    address
    state
  }
}
```

<details>

<summary>Sample Response</summary>

```json
{
  "data": {
    "wallet": {
      "id": "14",
      "address": "0xf253fc2cb42c078436d07fb77e5a76a649892172",
      "state": "READY"
    }
  }
}
```

</details>

#### [WalletState](https://api-docs-niftory.vercel.app/#definition-WalletState)

The Wallet's `state` is updated after each phase of wallet setup, as follows:

| State        | Description                                                                                                         | API Call                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| `UNVERIFIED` | The wallet has been registered, but not yet verified to belong to the signed-in user.                               | This is the state after the `registerClient` call. |
| `VERIFIED`   | The wallet is verified to belong to the signed-in user, but not yet ready to receive NFTs from this app's contract. | This is the state after the `verifyWallet` call.   |
| `READY`      | The wallet is ready to receive NFTs from this app's contract.                                                       | This is the state after the `readyWallet` call.    |

After the Wallet is marked as ready, it is fully operational in your application, and can send & receive NFTs!

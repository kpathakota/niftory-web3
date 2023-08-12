# Using Your API Key

Your API key is required for every request made to access content for your App. It should be passed as the `"X-Niftory-API-Key"` header. In addition, you'll receive a secret key, which provides full access to privileged actions on your behalf (minting, setting up wallets, querying users, etc). This should be passed in as `"X-Niftory-Client-Secret"`

This key identifies your application. It can be used on its own for any operation you'd want anyone visiting your application to be able to perform without logging in, like querying NFTs.

The API Key alone cannot be used to query any user or sensitive app data or perform any mutations. For that, see [Types of Authentication](./).

## Get Your Keys

Go to the [**Niftory Admin Portal**](https://admin.niftory.com/) and follow these steps:&#x20;

1. You should automatically be redirected to the Your App page. If not, click on your **User Profile,** and select **App.**&#x20;
2. Click [**Setup and Get Keys**](using-your-api-key.md) to get all of your secrets and start building. You're all set to go!&#x20;

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Niftory API Keys</p></figcaption></figure>

### Using Your API Keys

For simple commands (generally getting data like your NFTs, Wallets, etc), you can just use your API Key, which is safe to call for any application. For privileged commands, you'll use the Client Secret or our OAuth integrations. Read more in the [Privileged Authentication](privileged-authentication.md) section.

In our SDK samples, you'll usually see us creating a separate client for simple use-cases vs the privileged use-cases.&#x20;

<details>

<summary>Standard Niftory SDK Client (API Key Only)</summary>

```typescript
import { EnvironmentName, NiftoryClient, NiftoryProvider } from "@niftory/sdk"
import { useMemo } from "react"
import { useWalletContext } from "../hooks/useWalletContext"

export const NiftoryClientProvider = ({ children }) => {
  const client = useMemo(() => {
    return new NiftoryClient({
      environmentName: process.env.NEXT_PUBLIC_BLOCKCHAIN_ENV as EnvironmentName,
      appId: process.env.NEXT_PUBLIC_CLIENT_ID,
      apiKey: process.env.NEXT_PUBLIC_API_KEY,
    })
  }, [])

  return <NiftoryProvider client={client}>{children}</NiftoryProvider>
}
```

</details>

<details>

<summary>Privileged Niftory SDK Client (API Key + Client Secret)</summary>

```typescript
import { EnvironmentName, NiftoryClient } from "@niftory/sdk"
let client: NiftoryClient

/**
 * Gets a NIFTORY client for use in the backend.
 * @returns A NiftorySdk client.
 */
export function getBackendNiftoryClient() {
  client =
    client ||
    new NiftoryClient({
      environmentName: process.env.NEXT_PUBLIC_BLOCKCHAIN_ENV as EnvironmentName,
      appId: process.env.NEXT_PUBLIC_CLIENT_ID,
      apiKey: process.env.NEXT_PUBLIC_API_KEY,
      clientSecret: process.env.CLIENT_SECRET,
    })

  return client
}
```

</details>


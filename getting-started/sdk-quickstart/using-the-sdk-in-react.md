# Using the SDK in React

First, make sure you've installed the Niftory SDK on your project with:

```
yarn add @niftory/sdk
```

Now, you need to wrap your entire app with a `NiftoryProvider` and pass in an instance of `NiftoryClient` as follows:

```javascript
  import { NiftoryProvider } from "@niftory/sdk/react"
  import { NiftoryClient } from "@niftory/sdk"

  // _app.tsx
  const authToken = "YOUR_AUTH_TOKEN"
  const client = useMemo(() => {
    return new NiftoryClient({
      appId: process.env.NEXT_PUBLIC_CLIENT_ID,
      environmentName: process.env.NEXT_PUBLIC_ENV,
      apiKey: process.env.NEXT_PUBLIC_API_KEY,
      authToken,
    })
  }, [authToken])

  return (
    <NiftoryProvider client={client}>
     ....
    </NiftoryProvider>)
```

**Note**: This client created above uses [user auth](../../additional-topics/user-auth-client-side.md) and you need to pass in your users`authToken` after your user is authenticated. API calls that do not require user authorization (generally queries like `nftModels`) will still function if authToken isn't provided.\
\
**Important Note**: In typescript projects, to use the react imports (providers and hooks) you might need your `compilerOptions` in tsconfig `moduleResolution` to be set as `nodenext`

Now, you now call the exported react hooks as follows:

<pre class="language-javascript"><code class="lang-javascript">import { useWalletQuery, useRegisterWalletMutation, useNiftoryClient } from "@niftory/sdk/react"
<strong>
</strong><strong>const MyComponent = () => {
</strong>  // Query
  const [userWalletResponse, reExecuteQuery] = useWalletQuery()
  // Mutation
  const [{ data, fetching }, registerWallet] = useRegisterWalletMutation()
  // Use client directly
  const niftoryClient = useNiftoryClient()

  if (userWalletResponse.fetching) {
    return &#x3C;div>...Loading&#x3C;/div>
  }

  const { wallet } = userWalletResponse.data

  return (
    &#x3C;>
      {wallet &#x26;&#x26; &#x3C;p>Your wallet address is {wallet.address}&#x3C;/p>}{" "}
      {&#x3C;button onClick={() => registerWallet({ address: "0x123456" })}>Register Wallet&#x3C;/button>}
    &#x3C;/>
  )
}
</code></pre>

The hooks are always named in the format use[\[APIName\]](https://graphql.docs.niftory.com/#group-Operations-Queries)Query for queries and use[\[APIName\]](https://graphql.docs.niftory.com/#group-Operations-Mutations)Mutation for mutations. For APIName, you can view Niftory API docs [here](https://graphql.docs.niftory.com/).

For examples of react usage, you can check out our sample apps using the Niftory SDK:

[https://github.com/Niftory/niftory-samples](https://github.com/Niftory/niftory-samples)

# Using the SDK from the Server

First, make sure you've installed the Niftory SDK on your project with:

```
yarn add @niftory/sdk
```

Now use the SDK in your codebase as follows:

```javascript

import { NiftoryClient } from "@niftory/sdk"


...

const client =
    new NiftoryClient({
      environmentName: process.env.ENVIRONMENT //testnet or mainnet,
      appId: process.env.CLIENT_ID,
      apiKey: process.env.API_KEY,
      clientSecret: process.env.CLIENT_SECRET,
    })

await client.createNiftoryWallet()
```

**Note**: This will create a Niftory SDK with [privileged authentication](../../core-concepts/authentication/privileged-authentication.md) using a client secret that should never be public.&#x20;

### Parameters

<table><thead><tr><th width="188">Parameter</th><th width="224">Description</th><th>Example Value</th></tr></thead><tbody><tr><td>environmentName</td><td>Environment of the blockchain the app will run on.</td><td><code>testnet</code> or <code>mainnet</code></td></tr><tr><td>appId</td><td>App Id or Client id generated from the <a href="https://admin.niftory.com/">Niftory admin app</a></td><td><code>clfjx6jzq200gs915aiz7884q</code></td></tr><tr><td>apiKey</td><td>API key generated from the <a href="https://admin.niftory.com/">Niftory admin app</a></td><td><code>Xk0A3H/LjA1G1D99RU1L2zoS11vR1ZJs2m6Ncqu0tdA=</code></td></tr><tr><td>clientSecret</td><td>Client Secret generated from the <a href="https://admin.niftory.com/">Niftory admin app</a></td><td><code>40b27a45dtt6fce9b6e664613c43250aea67912wbfbcba263f1b57dce2c7b9ed6045f1d72dd3ca2f000070bc7a6af795c4c0c6fee70l0f3d2f290102b308aaba</code></td></tr></tbody></table>

All the available methods on the NiftoryClient are documented on the [API Reference](https://sdk.niftory.com/classes/NiftoryClient.html).



For examples of server-side usage you can check out our sample apps using the Niftory SDK:

[https://github.com/Niftory/niftory-samples](https://github.com/Niftory/niftory-samples)


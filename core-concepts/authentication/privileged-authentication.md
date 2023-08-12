# Privileged Authentication

Some operations require more privileged authentication — for example, if any user could invoke the [transfer](../nfts/transferring-nfts.md) mutation, they would be able to transfer as many NFTs as they wanted to themselves, so we probably only want the application to be able to initiate that operation!

For operations that should only be initiated from the app or app admin's context, we support two forms of privileged authentication.

### Backend Authentication

Backend authentication amounts to your application authenticating as itself, instead of in the AppUser context.

{% hint style="danger" %}
Backend authentication allows the App to perform any privileged operation against your application's resources.

**For this reason, it's extremely important to only use this kind of authentication in your backend.**
{% endhint %}

There are two ways of doing backend authentication - using your client secret or using OAuth.&#x20;

* Client Secret: Add your client secret header into the API Call (backend only).
* Open ID and OAuth: Authenticate using the [OAuth Client Credentials](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) grant. Many OAuth libraries support this.

The following snippets show you both options:

<details>

<summary>Using a Client Secret</summary>

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
}tu
```

</details>

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/lib/oauth.ts#L28">Using an Open ID-Client</a></summary>

```javascript
async function getOAuthClient() {
  if (
    !process.env.NEXT_PUBLIC_CLIENT_ID ||
    !process.env.CLIENT_SECRET ||
    !process.env.NIFTORY_AUTH_ISSUER
  ) {
    throw new Error(
      "NIFTORY_AUTH_ISSUER, NEXT_PUBLIC_CLIENT_ID, and CLIENT_SECRET must be set"
    );
  }

  if (!client) {
    const issuer = await Issuer.discover(process.env.NIFTORY_AUTH_ISSUER);
    client = new issuer.Client({
      client_id: process.env.NEXT_PUBLIC_CLIENT_ID,
      client_secret: process.env.CLIENT_SECRET,
    });
  }

  return client;
}

export async function getClientCredentialsToken() {
  const client = await getOAuthClient();

  if (!token || token.expired()) {
    token = await client.grant({ grant_type: "client_credentials" });
  }

  return token.access_token;
}
```

See [Configuring Your App](configuring-your-app.md#application-credentials) for details on these configuration values.

See the [Quick Start ](../../sample-app/niftory-sample-app/#get-your-api-keys)to get these properties for your app.

In this example, `NIFTORY_AUTH_ISSUER` should be the [Niftory Auth service endpoint](configuring-your-app.md#auth-service-endpoints), omitting the path since `openid-client` appends it by default.&#x20;

</details>

### Admin Authentication

In some situations, you may want members of your development team to log into your application and perform privileged operations. Most of these operations can be handled in the [Admin Portal](http://127.0.0.1:5000/o/ShoAj2x7X0erlYafyocL/s/Z0zX8NOUJGEW56P5Ijke/), but you can also authenticate your team members as AdminUsers instead.&#x20;

This works exactly like [User Authentication](privileged-authentication.md#user-authentication), but adding the `admin` scope to your OAuth configuration.

{% hint style="info" %}
Admin Authentication will only succeed if the user trying to log in is already a member of your team.
{% endhint %}

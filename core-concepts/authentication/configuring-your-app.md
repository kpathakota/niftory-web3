# Configuring Your App

{% hint style="info" %}
The Niftory Platform gives you the ability to create and manage users without using a separate authentication system (Auth0, Firebase, etc). This is fully optional - you're always free to bring your own auth system and interact with the API directly. \
\
The best way to set up user authentication is to look at the [Niftory Sample App](../../sample-app/anatomy-of-a-niftory-app/#authentication), and how it configures the Niftory auth provider.
{% endhint %}

Each application receives two credentials which can be exchanged for auth tokens:

### Client ID

The client ID is your application's username. It identifies your application and doesn't change.

### Client Secret

The client secret is your application's password. It's the secret that proves that your application is really making calls, and allows your application to authenticate itself and its users.&#x20;

{% hint style="danger" %}
**Never share your client secret with anyone, or use it directly in code. Never commit a `.env` containing this secret.**

**It should be kept hidden from your front-end and from your users.**
{% endhint %}

See the [Quick Start](../../sample-app/niftory-sample-app/#get-your-api-keys) to find out how to obtain these properties.

## Include your token with every API request

Whatever the [type of authentication](../../additional-topics/user-auth-client-side.md) you use, you'll end up with a bearer token.

[Configure your GraphQL client](../../sample-app/anatomy-of-a-niftory-app/#graphql-client) to pass this token in the `Authorization` header of every request. If you are using `next-auth`, you can retrieve the session token using the `useSession` hook.

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/lib/graphql/frontendClient.ts">Frontend graphQL Client with graphql-request</a></summary>

```javascript
/**
 * Creates a graphQL client for use in the browser, using the user's auth token for authentication
 * @param url The URL of the GraphQL API
 * @param apiKey The API key
 * @param session The user session
 * @returns The graphQL client
 */
export function getFrontendGraphQLClient(
  url: string,
  apiKey: string,
  session: Session | null
) {
  return new GraphQLClient(url, {
    headers: {
      "X-Niftory-API-Key": apiKey,
      Authorization: session?.authToken ? `Bearer ${session.authToken}` : "",
    },
  });
}
```

</details>

In the backend you can use [Backend Authentication](privileged-authentication.md#backend-authentication) to get a token, and then include it in your requests in the same way:

<details>

<summary>Backend GraphQL Client (using a Client Secret)</summary>

```
let client: GraphQLClient;

export async function getBackendGraphQLClient() {
  client =
    client ||
    new GraphQLClient(process.env.NEXT_PUBLIC_API_PATH, {
      headers: {
        "X-Niftory-API-Key": process.env.NEXT_PUBLIC_API_KEY,
        "X-Niftory-Client-Secret": process.env.CLIENT_SECRET,
      },
    });

  const token = await getClientCredentialsToken();

  client.setHeader("Authorization", `Bearer ${token}`);

  return client;
}
```

</details>

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/lib/graphql/backendClient.ts#L6-L20">Backend GraphQL client </a>(using OAuth)</summary>

```javascript
let client: GraphQLClient;

export async function getBackendGraphQLClient() {
  client =
    client ||
    new GraphQLClient(process.env.NEXT_PUBLIC_API_PATH, {
      headers: {
        "X-Niftory-API-Key": process.env.NEXT_PUBLIC_API_KEY,
      },
    });

  const token = await getClientCredentialsToken();

  client.setHeader("Authorization", `Bearer ${token}`);

  return client;
}
```

</details>

## Authentication Endpoints (OAuth Only)

Any authentication library that supports OAuth 2.0 can integrate the Niftory authentication provider.

The information you need to set this up varies depending on the authentication library you use. If your library supports [OIDC Discovery](https://openid.net/specs/openid-connect-discovery-1\_0.html), so you can simply point it to our well-known endpoints:

| Environment | URL                                                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Staging     | [https://auth.staging.niftory.com/.well-known/openid-configuration](https://auth.staging.niftory.com/.well-known/openid-configuration) |
| Production  | [https://auth.niftory.com/.well-known/openid-configuration](https://auth.staging.niftory.com/.well-known/openid-configuration)         |

Otherwise, you may need to check the above URLs to find our authorization endpoint and token endpoints.

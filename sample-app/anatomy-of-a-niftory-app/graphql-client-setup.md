# GraphQL Client Setup

The sample uses [graphql-request](https://www.npmjs.com/package/graphql-request) to interact with Niftory's GraphQL endpoints.&#x20;

{% hint style="info" %}
For production use-cases, we recommend using [urql](https://formidable.com/open-source/urql/docs/) or [Apollo](https://www.apollographql.com/docs/react/) clients.
{% endhint %}

{% hint style="info" %}
Revisit [our rationale](../../getting-started/api-cheat-sheet.md#why-start-with-graphql) for starting with GraphQL.
{% endhint %}

{% swagger method="post" path=" " baseUrl="https://graphql.api.niftory.com" summary="" %}
{% swagger-description %}
Your production GraphQL Endpoint for all Niftory Requests.

\




**Targets blockchain prod.**
{% endswagger-description %}

{% swagger-parameter in="header" name="X-Niftory-API-Key*" type="String" required="true" %}
Your Niftory API Key
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" %}
Bearer token containing the user's JWT. See 

[Authenticating the User](https://app.gitbook.com/o/ShoAj2x7X0erlYafyocL/s/1itXKRjyFqqWGYkUXFnP/~/changes/zm9EGxKfthcmKq8FJdUx/core-concepts/authentication/authenticating-the-user)

.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path=" " baseUrl="https://graphql.api.staging.niftory.com " summary="" %}
{% swagger-description %}
Your staging GraphQL Endpoint for all Niftory Requests. 

\




**Targets blockchain testnet.**
{% endswagger-description %}

{% swagger-parameter in="header" name="X-Niftory-API-Key*" type="String" required="true" %}
Your Niftory API Key
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" %}
Bearer token containing the user's JWT. See 

[Authenticating the User](https://app.gitbook.com/o/ShoAj2x7X0erlYafyocL/s/1itXKRjyFqqWGYkUXFnP/~/changes/zm9EGxKfthcmKq8FJdUx/core-concepts/authentication/authenticating-the-user)

.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/hooks/useGraphQLClient.tsx">GraphQL Client with graphql-request</a></summary>

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

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/b81c49ac38327926aabcfdee4cb5df564e046a03/basic-app/src/components/GraphQLClientProvider.tsx">GraphQL Client with URQL</a></summary>

```javascript
export function getGraphQLClient(
  url: string,
  apiKey: string,
  session: Session | null
) {
  return createClient({
    url: url,
    fetchOptions: {
      headers: {
        "X-Niftory-API-Key": apiKey,
        Authorization: session?.authToken ? `Bearer ${session.authToken}` : "",
      },
    },
  });
}

export const GraphQLClientProvider = ({
  children,
}: {
  children: React.ReactNode;
}) => {
  const { data: session } = useSession();

  const graphqlClient = getGraphQLClient(
    process.env.NEXT_PUBLIC_API_PATH as string,
    process.env.NEXT_PUBLIC_API_KEY as string,
    session
  );

  return <Provider value={graphqlClient}>{children}</Provider>;
};

```

</details>

No matter what client library you use, the 3 things you need to make requests to the Niftory API are:

* [URL of the Niftory API service](../../getting-started/api-cheat-sheet.md#api-endpoints)
* [Your app's API Key](../../core-concepts/authentication/using-your-api-key.md)
* [An auth token, for scenarios that require authentication](../../core-concepts/authentication/configuring-your-app.md#pass-the-session-token-with-every-api-request)

{% hint style="warning" %}
You can use the Niftory API in staging mode, which doesn't affect your production data, and executes blockchain transactions on **testnet**. The API endpoint you use -- `api.staging.niftory.com` vs. `api.niftory.com` determines the mode.

\
**We strongly recommend you use the API's staging endpoint until you are ready to deploy your app to production.**
{% endhint %}

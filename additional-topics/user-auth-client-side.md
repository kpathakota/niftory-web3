# User Auth (Client-Side)

{% hint style="info" %}
In any situation where you're calling our API with a Niftory AppUser, this is the type of authentication you should use. If you're using your own User system, you can skip this portion of the guide.&#x20;
{% endhint %}

To allow your users to sign in, set up their accounts, and interact with your application data, you'll have them log in via the [OAuth authorization code grant](https://www.oauth.com/oauth2-servers/server-side-apps/authorization-code/). This is the way most OAuth providers authenticate end users, and most OAuth libraries will handle the flow for you automatically.

By default, this will authenticate the user as an [AppUser](../core-concepts/app-and-appuser.md#appuser). This will allow them to see and manage their own data within your application.

We currently support login via Google. In the future, we will support other OIDC providers, and wallet-only login as well.

### NextAuth

In the sample app, user authentication is handled via [`next-auth`](https://next-auth.js.org/):

<details>

<summary><a href="https://github.com/Niftory/niftory-samples/blob/main/basic-app/pages/api/auth/[...nextauth].ts">Setting up the Niftory auth provider with NextAuth</a></summary>

```javascript
const NIFTORY_AUTH_PROVIDER: Provider = {
  id: "niftory",
  name: "Niftory",
  type: "oauth",
  wellKnown: urljoin(
    process.env.NIFTORY_AUTH_SERVICE as string,
    "/.well-known/openid-configuration"
  ),
  authorization: { params: { scope: "openid email profile" } },
  clientId: process.env.NEXT_PUBLIC_CLIENT_ID,
  clientSecret: process.env.CLIENT_SECRET,
  checks: ["pkce", "state"],
  idToken: true,
  profile(profile) {
    return {
      id: profile.sub,
      name: profile.name,
      email: profile.email,
      image: profile.picture,
    };
  },
  httpOptions: {
    timeout: 10000,
  }
};

export default NextAuth({
  providers: [NIFTORY_AUTH_PROVIDER],
  callbacks: {
    // Seealso: https://next-auth.js.org/configuration/callbacks
    jwt: async ({ token, user, account }) => {
      // user is only passed in at inital signIn.
      // Add authTime to token on signIn
      if (user) {
        token.authTime = Math.floor(Date.now() / 1000);
      }

      if (account?.id_token) {
        token.id_token = account.id_token;
      }

      return token;
    },
    session: async ({ session, token }) => {
      session.clientId = token.aud;
      session.userId = token.sub;
      session.authToken = token.id_token;

      return session;
    },
  },
});
```

Pay particular attention to&#x20;

```javascript
clientId: process.env.NEXT_PUBLIC_CLIENT_ID,
clientSecret: process.env.CLIENT_SECRET,
```

See [Application Credentials](user-auth-client-side.md#application-credentials) for more details on these properties.

</details>

This can be replaced by any other authentication system - everything is standard OAuth.&#x20;

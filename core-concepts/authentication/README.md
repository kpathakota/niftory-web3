# Authentication

{% hint style="info" %}
Follow [Anatomy of a Niftory App](../../sample-app/anatomy-of-a-niftory-app/#authentication) for a practical guide on setting up authentication for your application.
{% endhint %}

Niftory uses two forms of authentication to keep your application data safe:

### API Keys

Each application gets an API Key when registered with Niftory for project-level authentication. This key needs to be passed as `X-Niftory-API-Key` header alongside every API request. For privileged authentication (used for minting, wallet creation, transferring and more), you can either use the `X-Niftory-Client-Secret` header or use OAuth.

For more information, see below:

{% content-ref url="using-your-api-key.md" %}
[using-your-api-key.md](using-your-api-key.md)
{% endcontent-ref %}

{% hint style="info" %}
**Remember: Never share your client secret with anyone, or use it directly in code. Never commit a `.env` containing this secret.**

**It should be kept hidden from your front-end and from your users.**
{% endhint %}

### OAuth 2.0

Authentication and authorization can be hard in any application, especially in a space as confusing as web3. For any operations that require user or app authorization, we use [OAuth 2.0](https://oauth.net/2/).

Each application gets a client ID and a client secret from Niftory that can be used to authenticate the application itself, as well as its users, to use the Niftory API.



{% content-ref url="configuring-your-app.md" %}
[configuring-your-app.md](configuring-your-app.md)
{% endcontent-ref %}


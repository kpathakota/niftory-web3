# Niftory Quickstart

{% hint style="info" %}
Go through this **quickstart** before you start building with Niftory. If you're building in JS or TS, we recommend using the Niftory SDK to streamline your building.

You can also use our full GraphQL API for interacting with Niftory if you want deeper control over the queries, mutations or are going to be using Niftory in another language or framework (Unity, Python, etc). SDKs for other ecosystems are coming soon!
{% endhint %}

## Get your API Keys

Your API keys identify and protect your app; it's how Niftory recognizes who is making requests. To get your API keys,

1. Log in to the [Niftory Admin Portal](https://admin.niftory.com).
2. Enter a name for your organization (single word).hhin
3. Select your preferred blockchain. Currently we default to [Flow](https://flow.com/).

<div align="center">

<figure><img src="../.gitbook/assets/image (13).png" alt="Sign up to https://admin.niftory.com"><figcaption></figcaption></figure>

</div>

That's it! You will be redirected to Your App page, where your API Keys are waiting for you.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
These API keys target the testnet environment. We recommend you get your app working fully in testnet before switching to mainnet.
{% endhint %}

## Deploy your smart contract

Next up, let's get you a smart contract and deploy it to the blockchain! Niftory has built an advanced smart contract for your use. We'll skip the details in this section, but you can read more about it here:

{% content-ref url="../core-concepts/contract.md" %}
[contract.md](../core-concepts/contract.md)
{% endcontent-ref %}

We'll start with deploying it to testnet; as mentioned above, you should develop against testnet until you're ready to deploy your app.

1. Go to Your App page in the [Niftory Admin Portal](https://admin.niftory.com).
2. Enter a Contract Name (this is optional, by default your contract name will be your org name).
3. **Deploy Contract**

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

That was easy, right? In less than 5 minutes, you've been able to get your API keys, and deploy a smart contract to the blockchain!&#x20;

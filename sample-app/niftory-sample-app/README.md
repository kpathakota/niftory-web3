# Niftory Sample App

We have built a sample to showcase a basic working application that uses the Niftory API.&#x20;

### Try it out

You can play with a deployed instance of the sample app here:

{% embed url="https://sample.niftory.com" %}

You should be able to:

* Log in using Niftory's OAuth service.
* View the available NFT drops.
* Set up a Wallet.
* Mint and transfer an NFT to your wallet, and view your collection.

### Clone the Sample App

{% hint style="success" %}
First, [**get your API keys**](../../getting-started/api-quickstart/) from Niftory in order to start tinkering with the Sample App, and eventually getting your own app set up.
{% endhint %}

Clone the Niftory Sample App to start exploring the Niftory platform.

{% embed url="https://github.com/Niftory/niftory-samples" %}

#### Configure your .env

1. Copy the `.env.example` file in the repo to a `.env` file.
2. Update these environment variables to point to the API keys you got from [API Quickstart](../../getting-started/api-quickstart/).

| .env                     | Description               |
| ------------------------ | ------------------------- |
| NEXT\_PUBLIC\_API\_KEY   | API key for your app      |
| NEXT\_PUBLIC\_CLIENT\_ID | Client ID of your app     |
| CLIENT\_SECRET           | Client secret of your app |

{% hint style="warning" %}
Make sure to keep your `.env` in `.gitignore`. The CLIENT\_SECRET should never be shared with anyone (it's like a password for your app). [Learn more](../../core-concepts/authentication/configuring-your-app.md).
{% endhint %}

#### Set up your local environment

The Niftory Sample app is built on [NextJS](https://nextjs.org). You'll need a few things to get started, but if you've built a NextJS or React app before, it'll all be very familiar.&#x20;

{% hint style="info" %}
Follow the [**readme**](https://github.com/Niftory/niftory-samples/blob/main/basic-app/README.md) to finish setting up your local environment.
{% endhint %}

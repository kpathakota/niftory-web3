# Creating NFTs

{% hint style="info" %}
To create NFT content in a **no-code** manner, use the [Niftory Admin Portal](../../getting-started/niftory-admin-portal.md) -- the same place you got your API keys from. Follow this [guide for reference](https://docs.niftory.com/home/v/admin/guides/create-your-first-nft).
{% endhint %}

{% embed url="https://docs.niftory.com/home/v/niftory-admin/guides/create-your-first-nft" %}

To create NFT content using the API, follow the steps below:

### Create [NFTSet](https://graphql.docs.niftory.com/#definition-NFTSet)

If not already done so, create an NFTSet using the [`createNFTSet`](https://graphql.docs.niftory.com/#definition-NFTSet) API.

[**Try it out in Postman**](https://www.postman.com/dark-star-402968/workspace/niftory/request/22409520-77ed82c3-cabc-47a3-b4d5-501e26e2988b)

```graphql
mutation CreateSet($data: NFTSetCreateInput!) {
    createNFTSet(data: $data) {
        id
        title
        attributes
    }
}
```

Example args:

```json
{
    "data": {
        "title": "My Set",
        "attributes": {
            "properties": {
                "that": "are private to your app",
                "and": "don't get added to the blockchain"
            }
        }
    }
}
```

### Upload [NFTContent](https://graphql.docs.niftory.com/#definition-NFTContent)

There are two ways to upload content for your NFT. In both cases, Niftory provides you with pre-signed URLs to upload NFT content to.&#x20;

{% hint style="info" %}
Once you upload content to the pre-signed URL, by default Niftory automatically schedules upload to IPFS asynchronously.
{% endhint %}

#### [`uploadNFTContent`](https://graphql.docs.niftory.com/#mutation-uploadNFTContent)

[**Try it out in Postman**](https://www.postman.com/dark-star-402968/workspace/niftory/request/22409520-47f10f00-68a7-4808-9d74-eb4c5207cc6c)

This API creates an NFTContent object in the database, and sets the URL's to pre-signed URLs that you can upload a file and a corresponding poster to.

```graphql
mutation UploadNFTContent($name: String!, $description: String) {
    uploadNFTContent(name: $name, description: $description) {
        id
        files {
            id
            state
            name
            url
        }
        poster {
            id
            state
            url
        }
    }
}
```

```json
{
    "name": "Test file",
    "description": "This is a test file"
}
```

#### [`createFileUploadUrl`](https://graphql.docs.niftory.com/#mutation-createFileUploadUrl)

[**Try it out in Postman**](https://www.postman.com/dark-star-402968/workspace/niftory/request/22409520-75f3bfe8-8dbc-449f-a9cc-cfa937cb4f4a)

This gives a bit more control on what to upload. For e.g, if you just want to upload a file, but no corresponding poster, then this API is a good way to do that.&#x20;

You can also use this to just upload files without scheduling an IPFS upload. That can be achieved as follows:

```graphql
mutation CreateFileUploadUrl($name: String!, $description: String, $options: CreateFileOptionsInput!) {
    createFileUploadUrl(name: $name, description: $description, options: $options) {
        id
        name
        url
        state
    }
}
```

```json
{
    "name": "Test file",
    "description": "This is a test file",
    "options": {
        "uploadToIPFS": false
    }
}
```

**You can upload the files asynchronously.** That is, you can continue on to create an NFTModel before actually uploading the file.

### Create [NFTModel](https://graphql.docs.niftory.com/#definition-NFTModel)

[**Try it out in Postman**](https://www.postman.com/dark-star-402968/workspace/niftory/request/22409520-518200b4-5635-45d6-b325-bf7f062ef99d)

Finally, use the id of the NFTSet you created, as well as the id of the NFTContent to create an NFTModel:

```graphql
mutation CreateModel($setId: ID!, $data: NFTModelCreateInput!) {
    createNFTModel(setId: $setId, data: $data) {
        id
        quantity
        title
        attributes
    }
}
```

```json
{
    "setId": "f29f68ac-43a2-46da-b56b-01e3423719f5",
    "data": {
        "title": "My NFTModel",
        "description": "My NFT template",
        "quantity": 5,
        "contentId": "be040196-421c-48a8-8749-44edc7a92cdd",
        "metadata": {
            "this": "ends up on the blockchain"
        },
        "attributes": {
            "properties": {
                "that": "are private to your app",
                "and": "don't get added to the blockchain"
            }
        }
    }
}
```

And voila, you have your NFTModel!

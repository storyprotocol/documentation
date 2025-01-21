---
title: How to Register Music on Story
excerpt: >-
  Learn how to properly register music on Story as an IP Asset using the
  Typescript SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
In this tutorial, you will learn how to properly register music as IP on Story using the TypeScript SDK.

> 📢 Justin Bieber is coming to Story!
>
> "Peaches" by Justin Bieber is one of the first RWAs coming to Story. Check out the announcement [here](https://x.com/StoryProtocol/status/1881713146274156951)!

## First...

Most of this tutorial is covered in [How to Register IP on Story](doc:how-to-register-ip-on-story). Complete that tutorial first, and then come back here.

## Next...

The only difference is how you set your metadata. In your `ipMetadata`, you can set a few extra related parameters (`ipType`, `media`, `attributes`, `creators`) like so:

```typescript main.ts
const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
  title: 'My IP Asset',
  description: 'This is a test IP asset',
  ipType: 'Music',
  media: [
    {
      name: 'Rise Again',
      url: 'https://cdn1.suno.ai/937e3060-65c0-4934-acab-7d8cc05eb9a6.mp3',
      mimeType: 'audio/mpeg',
    },
  ],
  attributes: [
    {
      key: 'Artist',
      value: 'srivatsan_qb',
    },
    {
      key: 'Artist ID',
      value: '4123743b-8ba6-4028-a965-75b79a3ad424',
    },
    {
      key: 'Source',
      value: 'Suno.com',
    },
  ],
  creators: [
    {
      name: 'srivatsan_qb',
      address: account.address,
      contributionPercent: 100,
    },
  ],
})
```

In your `nftMetadata`, **in order for the music to actually be played on our explorer** you must set a `media` parameter, and then you can also set some `attributes` that look something like this:

```typescript main.ts
const nftMetadata = {
  name: 'Test NFT',
  description: 'This is a test NFT',
  image: 'https://picsum.photos/200',
  media: [
    {
      name: 'Rise Again',
      url: 'https://cdn1.suno.ai/937e3060-65c0-4934-acab-7d8cc05eb9a6.mp3',
      mimeType: 'audio/mpeg',
    },
  ],
  attributes: [
    {
      key: 'Artist',
      value: 'srivatsan_qb',
    },
    {
      key: 'Artist ID',
      value: '4123743b-8ba6-4028-a965-75b79a3ad424',
    },
    {
      key: 'Source',
      value: 'Suno.com',
    },
  ],
}
```

## Done!

When you run the script, you will register an IP Asset and it will look something like [this](https://explorer.story.foundation/ipa/0x0c299a18b96b14489409f58C2773413Fc4fb05d6) on our explorer.

You can see the explorer recognizes the metadata format, and you can play the song directly on the page!
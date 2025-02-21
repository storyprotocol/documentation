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
In this tutorial, you will learn how to properly register music as IP on Story using the TypeScript SDK. At the end, you will be able to listen to your song directly on our explorer.

<Cards columns={2}>
  <Card title="Example Final Result" href="https://aeneid.explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8" icon="fa-home" target="_blank">
    View an example result after following this tutorial.
  </Card>

  <Card title="Justin Bieber is coming to Story!" href="https://x.com/StoryProtocol/status/1881713146274156951" icon="fa-megaphone" target="_blank">
    "Peaches" by Justin Bieber is one of the first RWAs coming to Story. Check out the announcement!
  </Card>
</Cards>

## 1. Create a Song

Before we register music on Story, you'll obviously need some music! If you already have music, make sure you have a link to the music file directly. For example, `https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3`. If you don't already have this, you can upload your music file to IPFS:

If you want to create a test song, go to [Suno](https://suno.com), which is an awesome platform for AI-generated music. We can get a test song by:

1. Inputting a prompt to create a song
2. Click on the final result, which should take you to a URL like `https://suno.com/song/dcd3076f-3aa5-400b-ba5d-87d30f27c311`
3. Copy the the `SONG_ID` in the URL (`dcd3076f-3aa5-400b-ba5d-87d30f27c311`)
4. Copy the following URL: `https://cdn1.suno.ai/${SONG_ID}.mp3`, making sure to replace `SONG_ID` with your own.

This is the URL we'll use in step 2.

## 2. Complete the "How to Register IP" Tutorial

Most of what we need to do is already covered in [Register an IP Asset](doc:register-an-ip-asset). Complete that tutorial first, and then come back here.

## 3. Change Metadata

The only difference is how you set your metadata. Here is an example `ipMetadata`, where the `image.*` is the cover of the song, and `media.*` is the song itself.

> 📘 Infringement Check
>
> Note that the fields passed into `media.*` are checked for infringement by our protocol.

```typescript main.ts
const ipMetadata = {
  title: 'Midnight Marriage',
  description: 'This is a house-style song generated on suno.',
  createdAt: '1740005219',
  creators: [
    {
      name: 'Jacob Tucker',
      address: '0xA2f9Cf1E40D7b03aB81e34BC50f0A8c67B4e9112',
      contributionPercent: 100,
    },
  ],
  image: 'https://cdn2.suno.ai/image_large_8bcba6bc-3f60-4921-b148-f32a59086a4c.jpeg',
  imageHash: '0xc404730cdcdf7e5e54e8f16bc6687f97c6578a296f4a21b452d8a6ecabd61bcc',
  mediaUrl: 'https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3',
  mediaHash: '0xb52a44f53b2485ba772bd4857a443e1fb942cf5dda73c870e2d2238ecd607aee',
  mediaType: 'audio/mpeg',
}
```

In your `nftMetadata`, **in order for the music to actually be played on our explorer** you must set a `media` parameter, and then you can also set some `attributes` that look something like this:

* Make sure to replace the `media[0].url` with the one we created in step 0!

```typescript main.ts
const nftMetadata = {
  name: 'Midnight Marriage',
  description: 'This is a house-style song generated on suno. This NFT represents ownership of the IP Asset.',
  image: 'https://cdn2.suno.ai/image_large_8bcba6bc-3f60-4921-b148-f32a59086a4c.jpeg',
  media: [
    {
      name: 'Midnight Marriage',
      url: 'https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3',
      mimeType: 'audio/mpeg',
    },
  ],
  attributes: [
    {
      key: 'Suno Artist',
      value: 'amazedneurofunk956',
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

## 4. Done!

When you run the script, you will register an IP Asset and it will look something like [this](https://aeneid.explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8) on our explorer.

You can see the explorer recognizes the metadata format, and you can play the song directly on the page!
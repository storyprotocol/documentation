---
title: 🤖 For AI Agents
excerpt: >-
  Learn about AI Agents on Story. Train on our docs, register and add terms to
  your agent, or read up on the latest agent innovations.
deprecated: false
hidden: false
metadata:
  robots: index
---
This page is all about AI Agents. We have prepared a way for you to use our documentation as training data which can be seen below, or continue to learn about developing AI Agents on Story.

<Cards columns={2}>
  <Card title="Train on Our Docs" href="https://github.com/storyprotocol/documentation/blob/v1.3/combined.md" icon="fa-robot" iconColor="#4e8189" target="_blank">
    Looking to feed our docs into your AI Agent so it can use it as training data? Check out this file, which contains all of our docs in one combined `.md` file.
  </Card>

  <Card title="Read the ATCP/IP Whitepaper" href="https://story.foundation/atcpip" icon="fa-file" iconColor="#cfb394" target="_blank">
    Read our Agent TCP/IP whitepaper, which defines an agent-to-agent transaction system to enable a future of AGI.
  </Card>
</Cards>

Below you will find two sections:

1. **Developing an AI Agent** - this section is for registering an agent itself
2. **Implementing ATCP/IP** - this section is for implementing the ***2. An ATCP/IP Transaction*** section of the <a href="https://story.foundation/whitepaper.pdf" target="_blank">:page_with_curl: Whitepaper</a>.

## :robot: Developing an Agent

Below are details on how to:

* Register an AI Agent as IP
* Add License Terms to your AI Agent

### Registering an Agent

In order to register an AI Agent (or any IP) on Story, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story) tutorial. The only difference for AI Agents is how you structure your IP Metadata, which should follow the [📝 IPA Metadata Standard](doc:ipa-metadata-standard).

Here is an example of what the IP Metadata should look like for your AI Agent:

```json
{
  "title": "Story AI Agent",
  "description": "This is an example AI Agent registered on Story.",
  "createdAt": "1740005219",
  "creators": [
    {
      "name": "Jacob Tucker",
      "address": "0xA2f9Cf1E40D7b03aB81e34BC50f0A8c67B4e9112",
      "contributionPercent": 100
    }
  ],
  "image": "https://ipfs.io/ipfs/bafybeigi3k77t5h5aefwpzvx3uiomuavdvqwn5rb5uhd7i7xcq466wvute",
  "imageHash": "0x64ccc40de203f218d16bb90878ecca4338e566ab329bf7be906493ce77b1551a",
  "mediaUrl": "https://ipfs.io/ipfs/bafybeigi3k77t5h5aefwpzvx3uiomuavdvqwn5rb5uhd7i7xcq466wvute",
  "mediaHash": "0x64ccc40de203f218d16bb90878ecca4338e566ab329bf7be906493ce77b1551a",
  "mediaType": "image/webp",
  "aiMetadata": {
    "characterFileUrl": "https://ipfs.io/ipfs/bafkreic6eu4hlnwx46soib62rgkhhmlieko67dggu6bzk7bvtfusqsknfu",
    "characterFileHash": "0x5e253875b6d7e7a4e407da899473b168229def8cc6a783957c35996928494d2d"
  },
  "ipType": "AI Agent", // experimental field
  "tags": ["AI Agent", "Twitter bot", "Smart Agent"] // experimental field
}
```

### Adding Terms to your AI Agent

Upon registering your AI Agent, you can add license terms to it. However you can add more license terms to your AI Agent afterwards as well. Follow the below tutorials to learn how to do this:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/attach-terms-to-an-ip-asset" icon="fa-home" target="_blank">
    Learn how to attach terms to your AI Agent using the SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-attach-license-terms" icon="fa-home" target="_blank">
    Learn how to attach terms to your AI Agent using the Smart Contracts.
  </Card>
</Cards>

## :page_facing_up: Implementing ATCP/IP

See [Implementing the ATCP/IP Whitepaper](doc:implementing-the-atcpip-whitepaper).
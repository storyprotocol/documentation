---
title: AI Agents on Story
deprecated: false
hidden: false
metadata:
  robots: index
---
## Registering an Agent

<Cards columns={1}>
  <Card title="Example Agent - Benjamin" href="https://explorer.story.foundation/ipa/0x123C52537C98d6064bF7BbdC7D4A93A9D9c8E43A" icon="fa-home" target="_blank">
    View an example Agent here.
  </Card>
</Cards>

In order to register an AI Agent on Story, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story#/) tutorial. The only difference is how you structure your IP Metadata, which should follow the [IPA Metadata Standard](https://docs.story.foundation/docs/ipa-metadata-standard#/).

Here is an example of what the IPA Metadata should look like for your AI Agent (using Benjamin as an example):

```json
{
 	title: "Benjamin",
  description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
  ipType: "AI Agent",
  creators: [
    {
 			name: "Unleash Protocol",
      address: "0xBBE1725E1Fef9C2C20Ac6CB984bB0E970Ab18aa9",
      contributionPercent: 100,
      description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
      socialMedia: [{ platform: "twitter", url: "https://x.com/BenjaminOnIP" }]
    }
  ],
  tags: ["Benjamin", "Unleash Protocol", "Story", "AI Agent", "IPFI"],
}
```
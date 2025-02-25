---
title: 🕵️ Story Attestation Service
excerpt: A multi-layered decentralized approach to validating intellectual property.
deprecated: false
hidden: false
metadata:
  robots: index
---
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  You can think of the Story Attestation Service (SAS) as a bunch of independent service providers (infringement, identity, etc) each proving the validity of an IP in their own way. So that each IP has a set of "badges" on it displaying the results.

  It's then up to the ecosystem/market to determine which providers they trust or want to believe. This becomes a  decentralized "validator"-like approach to IP validity, where if an IP Asset has lots of providers saying it is valid, then it probably valid.
</Accordion>

Story employs a multi-layered decentralized approach to validating intellectual property, grounded in two foundational components:

1. The Story Attestation Service (SAS): leverages a network of specialized service providers — each detecting copyright violations across different mediums (images, audio, etc) — to provide transparent, publicly accessible signals on the legitimacy of an [🧩 IP Asset](doc:ip-asset). Applications that facilitate IP registration (e.g. original content) may also attest to the provenance of an IP asset (called "apptestations") in the future.
2. The [❌ Dispute Module](doc:dispute-module): offers a flexible framework for resolving conflicts, tapping both on-chain and off-chain processes to accommodate the nuanced nature of IP disputes.

This blend of detection methods and dispute resolution creates a robust ecosystem that allows IP to be registered without introducing undue friction, while letting the market, and individual ecosystem apps, determine how much weight to give each attestation provider.

These layers make up the **IP Validation Service (IPVS)** - a fully decentralized marketplace of trust. The existing system of detection providers will continue to expand into a broader ecosystem of signal contributors, each able to offer specialized, verifiable assessments of IP authenticity. Through incentivized participation, IPVS fosters a self-sustaining market where different validators collaborate to deliver specialized signals.

So rather than preventing duplicates, which would cause far more potentially disruptive front running risk, the signals and attestations allow the original IPs surface above the rest.

## "When does the SAS scan my IP?"

> 📘 Quick Note
>
> It's important to note that the Story Attestation Service only runs IP infringement checks on **commercial IP**. That is, IP Assets who have at least one [License Terms](doc:license-terms) where `commercialUse = true`.
>
> If your IP is non-commercial, then this section doesn't apply to you.

When [registering your IP on Story](doc:how-to-register-ip-on-story), you pass in IP-specific metadata that implements the [📝 IPA Metadata Standard](doc:ipa-metadata-standard). In this standard, you'll see 3 fields:

| Property Name | Type     | Description                                                                                                                                                                                                                                    |
| :------------ | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mediaUrl`    | `string` | Used for infringement checking, points to the actual media (ex. image or audio)                                                                                                                                                                |
| `mediaHash`   | `string` | Hashed string of the media using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content) for how that is done.                                                                         |
| `mediaType`   | `string` | Type of media (audio, video, image), based on [mimeType](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types). See the allowed media types [here](https://docs.story.foundation/docs/ipa-metadata-standard#media-types). |

These are used for the commercial infringement check. Whatever media you pass in through `mediaUrl` will be checked by our infringement detection providers and flagged if infringement is detected.

If you do not pass in these `media.*` fields, then an infringement detection will not be performed and your IP will not be proven valid.

### Current Limitations

* You must set the `media.*` fields before attaching commercial terms (`commercialUse = true`), otherwise no check will be performed.
* Attestations will only show up on the IP Portal (our "GitHub for IP" platform coming soon). We are working on publishing attestations to public record so anyone can access the results (**COMING SOON!**).
* Only media that is **existing on the internet** will be detected. If someone registers new IP on Story, it will simply return validated because our providers don't have data on it.

## Current Providers

<Cards columns={2}>
  <Card title="Yakoa" href="https://www.yakoa.io/" icon="fa-home" iconColor="#190087" target="_blank">
    Yakoa uses AI and machine learning to scan multiple blockchains, analyzing on-chain data to detect direct copies, stylistic forgeries, and unauthorized replications of digital assets. It compares new assets against a database of known IP, flagging potential violations in real time and providing detailed audit logs for enforcement.
  </Card>

  <Card title="Pex" href="https://www.pex.com/" icon="fa-home" iconColor="#019cf4" target="_blank">
    Pex.com is a digital platform that leverages advanced content recognition and analytics to help creators and rights holders track, manage, and monetize their visual and audio media online. It monitors how content is used across the web, making it easier for users to discover licensing opportunities and protect their intellectual property.
  </Card>
</Cards>

## Becoming an Attestation Provider

The Story Attestation Service is undergoing active development. If you run any form of IP validation (infringement, identity, origin, etc), then you can become an attestation provider. To do so, please fill out this [form](https://docs.google.com/forms/d/10n3AnWoiLsxpaY17kJlxRazysDe8aOWJgirRnfkFRAk/edit).
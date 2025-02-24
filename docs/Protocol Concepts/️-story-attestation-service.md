---
title: 🕵️ Story Attestation Service
excerpt: A multi-layered decentralized approach to validating intellectual property.
deprecated: false
hidden: false
metadata:
  robots: index
---
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  You can think of the Story Attestation Service (SAS) as a bunch of independent infringement detection providers each proving the validity of an IP in their own way. So that each IP has a set of "badges" on it displaying who thinks the IP is valid and who doesn't.

  It's then up to the ecosystem/market to determine which providers they trust or want to believe. This becomes a very decentralized "validator"-like approach to IP validity, where if an IP Asset has tons of providers saying it's valid, then it's probably valid.
</Accordion>

Story employs a multi-layered decentralized approach to validating intellectual property, grounded in two foundational components:

1. The Story Attestation Service (SAS): leverages a network of specialized service providers — each detecting copyright violations across different mediums (images, audio, etc) — to provide transparent, publicly accessible signals on the legitimacy of an [🧩 IP Asset](doc:ip-asset). Applications that facilitate IP registration (e.g. original content) may also attest to the provenance of an IP asset (called "apptestations") in the future.
2. The [❌ Dispute Module](doc:dispute-module): offers a flexible framework for resolving conflicts, tapping both on-chain and off-chain processes to accommodate the nuanced nature of IP disputes.

This blend of detection methods and dispute resolution creates a robust ecosystem that allows IP to be registered without introducing undue friction, while letting the market, and individual ecosystem apps, determine how much weight to give each attestation provider.

These layers make up the **IP Validation Service (IPVS)** - a fully decentralized marketplace of trust. The existing system of detection providers will continue to expand into a broader ecosystem of signal contributors, each able to offer specialized, verifiable assessments of IP authenticity. Through incentivized participation, IPVS fosters a self-sustaining market where different validators collaborate to deliver specialized signals.

So rather than preventing duplicates, which would cause far more potentially disruptive front running risk, the signals and attestations allow the original IPs surface above the rest.

## Proving Validity of Your IP

You may be wondering, "How do I actually prove my IP is valid?"

> 📘 Quick Note
>
> It's important to note that we only perform IP infringement checks on **commercial IP**. That is, IP Assets who have at least one [License Terms](doc:license-terms) where `commercialUse = true`.
>
> If your IP is non-commercial, then this section doesn't apply to you.

When [registering your IP on Story](doc:how-to-register-ip-on-story), you also pass in IP-specific metadata that implements the [📝 IPA Metadata Standard](doc:ipa-metadata-standard). In this standard, you'll see 3 fields:

| Property Name | Type     | Description                                                                                                                                                                                                                                    |
| :------------ | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mediaUrl`    | `string` | Used for infringement checking, points to the actual media (ex. image or audio)                                                                                                                                                                |
| `mediaHash`   | `string` | Hashed string of the media using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content) for how that is done.                                                                         |
| `mediaType`   | `string` | Type of media (audio, video, image), based on [mimeType](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types). See the allowed media types [here](https://docs.story.foundation/docs/ipa-metadata-standard#media-types). |

These are used for the commercial infringement check. Whatever media you pass in through `mediaUrl` will be checked by our infringement detection providers and flagged if proven malicious.

If you do not pass in these `media.*` fields, then an infringement detection will not be performed and your IP will not be proven valid.

## Current Providers

1. [Yakoa](https://www.yakoa.io/) - Yakoa uses AI and machine learning to scan multiple blockchains, analyzing on-chain data to detect direct copies, stylistic forgeries, and unauthorized replications of digital assets. It compares new assets against a database of known IP, flagging potential violations in real time and providing detailed audit logs for enforcement.
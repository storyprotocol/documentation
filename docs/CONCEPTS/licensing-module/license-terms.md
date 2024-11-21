---
title: License Terms
excerpt: >-
  A particular combination of values from a License Template that define how
  others can interact with your IP.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> 🍦 Example License Terms
>
> View some popular combinations of PIL License Terms, also known as "flavors", [here](https://docs.story.foundation/docs/pil-flavors#/).

License Terms are a particular combination of values from a [License Template](doc:license-template). Indeed, there can and will exist **multiple** License Terms (variations) for each License Template. You can imagine that a License Template generates many License Term variations.

<Image align="center" src="https://files.readme.io/62ee532-Screenshot_2024-05-07_at_17.59.18.png" />

Once registered, **License Terms are immutable — they can't be tampered with or altered**, even by the License Template that generated it.

Additionally, License Terms have a unique numeric ID within the License Template they stem from. This makes License Terms reusable, meaning if someone creates License Terms with a specific set of values, it only needs to be created once and can be used by anyone else.

For example, a particular set of term values of the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil), such as non-commercial usage + derivatives allowed + free minting, defines a unique License Terms with an associated ID.

## License Terms Attached to IP Asset

The owner of a root IP Asset can attach License Terms to signal to other users that they can mint License Tokens of those terms to create a derivative of this IP Asset. **Once License Terms are attached to an IP Asset, it is now considered "public" and anyone can mint a License Token using those terms.**

<Image align="center" src="https://files.readme.io/39a365f-Screenshot_2024-05-07_at_18.43.38.png" />

## Inherited License Terms

On the other hand, derivative IP Assets inherit their License Terms from the parent IP Asset. This means that when an IP Asset registers itself as a derivative, it burns the License Token and inherits the associated License Terms. **The owner of this derivative cannot set new License Terms.**

> 📘 Changing Certain License Terms on a Derivative
>
> You may be wondering: "if I cannot set new License Terms on my derivative, does that also mean I can't change the minting fee, or disallowing more derivatives, on my derivative?"
>
> Thankfully, there is a way to get around this! Although you cannot change License Terms on a derivative IP, you can utilize the [License Config to implement special behaviors](doc:license-config-hook).

## Expiration

License Terms support an `expiration` time. Once License Terms expire, any derivatives that abide by that license will no longer be able to generate revenue or create further derivatives. If an IP Asset is a derivative of multiple parents, it will expire when the soonest expiration time between the two parents is reached.
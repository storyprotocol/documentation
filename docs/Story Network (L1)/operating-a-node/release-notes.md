---
title: Release Notes
deprecated: false
hidden: false
metadata:
  robots: index
---
This page provides information on the story execution and consensus client software release information. You may find execution client releases in [story-geth](https://github.com/piplabs/story-geth/releases) repo, and consensus client releases in [story](https://github.com/piplabs/story/releases) repo.

### Production releases

There are generally four types of releases:
* Major: It requires hardfork upgrade with a predefined upgrade height. Node operators need to upgrade before or on the height. The release will increase minor version number.
* Minor: It doesn't require hardfork upgrade. Node operators are required to upgrade binaries as soon as possible. The release will increase patch version number.
* Fix: It is an urgent fix. Node operators are required to upgrade binaries as soon as possible. The release will increase minor version or patch version number.
* Optional: It is an optional fix. Node operators can upgrade binaries based on needs. The release will increase patch version number.

Each release comes with a release note describing a list of new features or fixes. Released software binaries are also attached in the release note. We currently provide binaries supporting four types of systems: darwin-amd64, darwin-arm64, linux-amd64, and linux-arm64. You may also build your binaries using the commit hash in the release note.

### Release entries

Refer to the following release matrix to run nodes for Mainnet and Aeneid Testnet.

| Network | story-geth | story  |
|---------|------------|--------|
| Mainnet	| v1.0.1	   | v1.1.0 |
| Aeneid 	| v1.0.1	   | v1.1.0 |
---
title: React SDK Setup
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Prerequisite

For running the tutorial for developers, we require node version 20 or later version and npm version 8 to be installed in your environment. To install node and npm, we recommend you go to the [Node.js official website](https://nodejs.org) and download the latest LTS (Long Term Support) version.

If you want to upgrade node to 20 or higher version, we recommend you to use [nvm](https://github.com/nvm-sh/nvm) (Node Version Manager). If you want to upgrade npm to the latest version, you can run the command `npm install -g nvm`.

# Create a Next.js Project

Create a folder called `story-react-example`.

Then follow the short [Next.js "Getting Started: Installation"](https://nextjs.org/docs/getting-started/installation#automatic-installation) guide to set up a boilerplate Next.js project.

# Initiate SDK Provider

You can set up the `StoryProvider`, which is needed for the React SDK, any way you like. However we have provided three popular examples using [Wagmi](https://wagmi.sh/) and either [Dynamic](https://www.dynamic.xyz/), [RainbowKit](https://www.rainbowkit.com/)  or [WalletConnect](https://walletconnect.com/) based on your preference.

## [Wagmi + Dynamic Setup](doc:wagmi-dynamic-setup)

## [Wagmi + RainbowKit Setup](doc:wagmi-rainbowkit-setup)

## [Wagmi + WalletConnect Setup](doc:wagmi-walletconnect-setup)
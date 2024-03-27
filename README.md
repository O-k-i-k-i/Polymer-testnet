# ⛓️🔗⛓️ Template for IBC enabled Solidity contracts

This repo provides a starter project to build [IBC](https://github.com/cosmos/ibc) enabled Solidity contracts that connect rollups to one another Polymer Hub, through the [vIBC core contracts](https://github.com/open-ibc/vibc-core-smart-contracts).

The repository is a _GitHub template_ repository so you can click "Use this template" to create your own project repository without having the entire commit history of the template.

![GitHub template](./img/gh_template.png)

## 📚 Documentation

There's some basic information here in the README but a more comprehensive documentation can be found in [the official Polymer documentation](https://docs.polymerlabs.org/docs/category/build-ibc-dapps-1).

## 📋 Prerequisites

The repo is **compatible with both Hardhat and Foundry** development environments.

- Have [git](https://git-scm.com/downloads) installed
- Have [node](https://nodejs.org) installed (v18+)
- Have [Foundry](https://book.getfoundry.sh/getting-started/installation) installed (Hardhat will be installed when running `npm install`)
- Have [just](https://just.systems/man/en/chapter_1.html) installed (recommended but not strictly necessary)

Some basic knowledge of all of these tools is also required, although the details are abstracted away for basic usage.

## 🧰 Install dependencies

To compile your contracts and start testing, make sure that you have all dependencies installed.

From the root directory run:
```bash
just install
```
to install the [vIBC core smart contracts](https://github.com/open-ibc/vibc-core-smart-contracts) as a dependency. 

Additionally Hardhat will be installed as a dev dependency with some useful plugins. Check `package.json` for an exhaustive list.

## ⚙️ Set up your environment variables

Convert the `.env.example` file into an `.env` file. This will ignore the file for future git commits as well as expose the environment variables. Add your private keys and update the other values if you want to customize (advanced usage feature).

```
Update:     .env.example -------> .env
```

This will enable you to sign transactions with your private key(s). If not added, the scripts from the justfile will fail.

## 🏃🏽🏃🏻‍♀️ Quickstart

The project comes with a built-in dummy application called x-counter. You can find the contracts in the `/contracts` directory as XCounterUC.sol and XCounter.sol (the former when using the universal channel, the latter when creating a custom IBC channel).

The default setup (`.env`, `config.json`) are preconfigured to try to send packets over the universal channel.

Run the following as a sanity check:
```bash
# Usage: just send-packet [source] [universal]
# Source can be either 'optimism' or 'base', universal is set to true
just send-packet optimism true
```
<!-- TODO: add how to check for the packet on explorer or set up an event listener -->

Check if the packet got through on the [Polymer IBC explorer](https://explorer.prod.testnet.polymer.zone/packets).

## 💻 Develop your custom application

The main work for you as a developer is to develop the contracts that make up your cross-chain logic.

You can use the contracts in the "/contracts/base" directory as base contracts for creating IBC enabled contracts that can either send packets over the universal channel or create their own channel to send packets over.

A complete walkthrough on how to develop these contracts is provided in the [official Polymer documentation](https://docs.polymerlabs.org/docs/build/ibc-solidity/).

## 🕹️ Interaction with the contracts

When the contracts are ready, you can go ahead and interact with the contracts through scripts. There is a Justfile to for the most common commands, with the underlying scripts in the /scripts folder.

There's three types of default scripts in the project:

- `deploy.js` and `deploy-config.js` allow you to deploy your application contract
- `create-channel.js` and `create-channel-config.js` creates a channel
- `send-packet.js` sends packets over an existing custom channel, and `send-universal-packet.js` is specifically for sending packets over a universal channel

For every script you'll find a field in the configuration file!!

> **Note**: These are the default scripts provided. They provide the most generic interactions with the contracts to deploy, create channels and send packets. For more complicated use cases you will want to customize the scripts to your use case. See [advanced usage](#🦾-advanced-usage) for more info.

### Deploy

Before deploying, make sure to update the config.json with your contract type:
```json
  ...
  "deploy": {
    "optimism": "YourNewContract",
    "base": "YourNewContract"
  },
  ...
```

Then run:
```bash
# Usage: just deploy [source] [destination] [universal]
just deploy optimism base true
```
for an application that will use a universal channel, or:
```bash
# or 
just deploy optimism base false
```
for an application that uses custom channels.

The script will take the output of the deployment and update the config file with all the relevant information.

### Create a channel

If you're **using universal channels, channel creation is not required**. Your contract will send and receive packet data from the Universal channel handler contract which already has a universal channel to send packets over. You can directly proceed to sending (universal) packets in that case.

To create a custom channel, run:
```bash
just create-channel
```

This creates a channel between base and optimism. Note that the **ORDER MATTERS**; if you picked optimism as the source chain (first argument) above, by default it will create the channel from optimism and vice versa.

The script will take the output of the channel creation and update the config file with all the relevant information.

Check out the [channel tab in the explorer](https://explorer.prod.testnet.polymer.zone/channels) to find out if the correct channel-id's related to your contracts were updated in the config.

### Send packets
Finally Run:
```bash
# Usage: just send-packet [source] [universal]
just send-packet optimism true
```
to send a packet over a **universal channel**. You can pick either optimism or base to send the packet from.

Or run:
```bash
just send-packet optimism false
```
to send a packet over a **custom channel**. You can pick either optimism or base to send the packet from.

## 🦾 Advanced usage

For advanced users, there's multiple custimizations to follow. These includes configuring the config.json manually and/or running the scripts without using just.

For example, the last action to send a packet on a universal channel could be executed with this command:
```bash
npx hardhat run scripts/send-universal-packet.js --network base
```

To send a universal packet from the contract specified in the config.sendUniversalPacket field in the config.

## 🤝 Contributing

We welcome and encourage contributions from our community! Here’s how you can contribute.

Take a look at the open issues. If there's an issue that has the _help wanted_ label or _good first issue_, those are up for grabs. Assign yourself to the issue so people know you're working on it.

Alternatively you can open an issue for a new idea or piece of feedback.

When you want to contribute code, please follow these steps:

1. **Fork the Repository:** Start by forking this repository.
2. **Apply the improvements:** Want to optimize something or add support for additional developer tooling? Add your changes!
3. **Create a Pull Request:** Once you're ready and have tested your added code, submit a PR to the repo and we'll review as soon as possible.

## 💡 Questions or Suggestions?

Feel free to open an issue for questions, suggestions, or discussions related to this repository. For further discussion as well as a showcase of some community projects, check out the [Polymer developer forum](https://forum.polymerlabs.org).

Thank you for being a part of our community!
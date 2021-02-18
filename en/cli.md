---
title: Getting Started
description: To use the various ecosystems CastHub provides to Developers, we offer an official CLI tool
position: 2
category: CLI
---

To use the various ecosystems CastHub provides to Developers, we offer an official CLI tool that can use [used in a CI/CD environment](/cli/ci), or locally for ease-of-use.

## Installation

CastHub CLI is available via NPM/Yarn, and can be installed globally or locally, based on your workflow.

<code-group>
<code-block label="Yarn" active>

```bash
yarn global add @casthub/cli
```

</code-block>
<code-block label="NPM">

```bash
npm install -g @casthub/cli
```

</code-block>
</code-group>

If you install globally, ensure you have the NPM/Yarn `bin` in your `$PATH` if you are told the command could not be found:

- [Yarn](https://classic.yarnpkg.com/en/cli/global/#adding-the-install-location-to-your-path)
- [NPM](https://nodejs.dev/learn/where-does-npm-install-the-packages)

## Authentication

Most CLI commands require authentication to work, such as uploading assets and creating new content versions. This is done using a **Personal Access Token**. These are authentication tokens that _don't expire_, and grant complete access to your published content. As such, you should protect them like you would a password, and never store them in repositories.

To generate a Personal Access Token, you'll need to [download and install](https://casthub.app/download) the CastHub App, [activate Developer Tools](/#enabling-developer-features) and navigate to the Developers section. From here, there is a `Personal Access Tokens` tab.

When creating a Personal Access Token, you'll be given a list of available scopes. For the purposes of the CLI (Whether for use on your own machine or in CI/CD), you should **only** select the `cli` scope. This scope will grant the token access to only the areas of your Account that are needed by the CLI tool.

Once you have your Personal Access Token (**Remember**, you'll only ever see it after creation for security purposes), you can tell the CLI to use that token:

```sh
$ casthub token MY_TOKEN
```

---
title: Getting Started
description: ''
position: 6
category: Modules
---

The CastHub Dashboard System is built around individual Modules, each of them doing their own thing with a helping-hand from the CastHub Ecosystem.

## What makes a Module

Modules are, at their most basic level, a single element defined using the <a href="https://developer.mozilla.org/en-US/Web/Web_Components" target="_blank" rel="noopener noreferrer">Web Components API</a>.

Web Components are _super_ cool, and Modules use them to their full capacity, automating some bits and pieces for you, such as enforcing use of the Shadow DOM for HTML & CSS encapsulation.

Don't know anything about Web Components? Not to worry, we have a bunch of <a href="https://github.com/casthub" target="_blank" rel="noopener noreferrer">open-source</a> Modules for you to have a look at, as well as examples throughout these docs.

## Module Discovery

Users download and install Modules via the Module Store. Unlike conventional App-like Stores, there are currently no restrictions on submissions, though this is subject to change in the future.

## Creating a Module

Modules are created and managed within the CastHub App itself, so if you haven't already, [download the app](/download).

Once downloaded, installed and signed-in, you're ready to create your Module. Go to "Account Settings" via the <app-icon type="settings"></app-icon> icon and enable **Developer Tools**. You'll now have access to the Developer section of the App via the <app-icon type="code"></app-icon> icon!

### Filling out the Form

No-one likes forms, we know. We've tried to keep it as short and to-the-point as possible. This is just a quick run-down on some potentially confusing parts of the form for a new Module Developer:

- **Module Key** - Think of this like your Element Tag. For example, `hello-world`. This is unique for all Modules and cannot be changed after creation.
- **Requires Identity** - If `true`, when a User installs your Module, they must pick an Identity to give your Module access to. If left unchecked, they won't be prompted and you won't be given access to any of their Identities.
- **Supported Integrations** - This is just the third-party Integrations your Module plans to use. If you chose to require an identity (as read above), the third-party Accounts the User can choose from will be limited to what you check here.

Once you're done defining your Module, continue to create it. Don't worry - only the **Module Key** is permanent - everything else can be changed in the future.

## Template Scaffolding

Once you successfully create a Module, you'll find a new folder in your local App Data that is scaffolded and ready-to-go for your new Module. You can find the location of this folder based on your Operating System:

- **Windows** - `C:\Users\{user}\AppData\Roaming\CastHub\modules`
- **Linux** - `~/.config/CastHub/modules`

Or by clicking the <app-icon type="folder"></app-icon> Button when viewing the Module page.

The folder generated for your Module will be the same as your `key`.

### index.js

All Modules have a single export point, and in this scaffold it is `index.js`. The name of the file isn't important, as long as the `main` in your `package.json` points to it (See <docs-link path="/modules/metadata">Metadata</docs-link> for more on this)

The returned class in your `index.js` **must** be a subclass of `window.casthub.module`. The <docs-link path="/modules/stock-elements">Stock Elements</docs-link> all inherit from this base class, so you are also able to extend them directly instead.

## Using Unpublished Modules

Unpublished Modules will not appear inside the Module Store and, as such, you cannot add them to your Dashboards the conventional way.

First, make sure you have a Dashboard ready. It is recommended to have a Dashboard made purely for Development purposes. Once ready, follow these instructions:

1. Go to the Developers section (You can find itn by clicking your avatar in the titlebar of the App)
2. Click the "My Modules" Tab
3. Click in to your Module
4. Click the blue "Install" button

And there you go, your unpublished Module is now on your Dashboard ready to develop!

## Structure

Modules are single <a href="https://developer.mozilla.org/en-US/Web/Web_Components" target="_blank" rel="noopener noreferrer">Web Component</a> that must always extend the `window.casthub.module` Component, or a <docs-link path="/modules/stock-elements">subset of it</docs-link>.

With this comes some useful methods to help in working with the Shadow DOM and other aspects of Module Development.

### HTML

Modules can supply a Template HTML File that will be injected and available to the Module at run-time. You can <docs-link path="/modules/html">read the Documentation here</docs-link>.

Otherwise, you are free to add and remove Elements from the top-level container as you see fit, for example:

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.module {
    constructor() {
        super();

        // Create a new Element.
        this.$newElement = document.createElement('div');
        this.$newElement.classList.add('hello-world');

        // And add it to the Module Container.
        this.addEl(this.$newElement);
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.module {
    constructor() {
        super();

        // Create a new Element.
        this.$newElement = document.createElement('div');
        this.$newElement.classList.add('hello-world');

        // And add it to the Module Container.
        this.addEl(this.$newElement);
    }
};
```

</code-block>
</code-group>

Some useful references you have access to are:

- `this.$container` - The overall container of the Module Element.

### CSS

There are many ways of customizing your Module's CSS - they are all covered in our <docs-link path="/modules/css">CSS Documentation</docs-link>.

## Flow and Debugging

Modules are actually individual processes, allowing among other things for them to have their own individual Devtools.

If you have Developer Tools enabled, each Module will have an `Inspect` option when you click the <app-icon type="settings"></app-icon> icon on the Module.

### Refreshing Modules

Since a Module is loaded in to memory when the App boots, it's somewhat tricky to develop without refreshing the entire App every time you want to see your changes.

To combat this, we hook in to refreshes for your Module - so when you refresh the Module, we load it from the filesystem again and re-inject it in to the Module process.

This can be achieved by refreshing the Devtools window for your Module (`F5` or `[CMD/CTRL] + R`)

## Advanced Boilerplate

The basic Module Scaffolding offered during Module creation can be hard to work with if you're not creating a small, simple Module. As such, we have built an Advanced Boilerplate that offers the following:

- Pre-configured Building using Rollup
- SCSS pre-processing
- Direct CSS imports
- Automatic publishing & uploading to CastHub

The Advanced Boilerplate is available <a href="https://github.com/casthub/boilerplate-module" target="_blank" rel="noopener noreferrer">on our GitHub</a> with additional usage instructions.

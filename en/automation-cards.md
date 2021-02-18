---
title: Getting Started
description: The CastHub Automation system allows for users to set-up automated workflows using Cards
position: 15
category: Automation Cards
---

The CastHub Automation system allows for users to set-up automated workflows using Cards.

## Types

CastHub Automations are built around individual Cards, each being 1 of 3 types; **Triggers**, **Conditions**, and **Actions**.

### Triggers

Trigger Cards are the start of the workflow - using the [vast range of available Integrations](/third-party), or even custom logic, these Cards will trigger a run of the entire Automation lifecycle.

### Conditions

Once a Trigger Card has started the Automation lifecycle, Condition Cards are executed. These Cards will all run at the same time - and only if **all** the Cards are ran without any Errors thrown will the Automation continue to run.

### Actions

Once triggered, and given all Conditions have passed, Action Cards are executed. These Cards perform certain actions - such as posting a Tweet - and receive any [input](/automation-cards/io) from the Trigger Card that initiated the Automation.

## Creating a Card

Ensure you have [enabled Developer Tools](/#enabling-developer-features) and then head to the Developers section of the App. In here, there is an `Automation Cards` tab where you can create a new Card.

- **Card Type** - As discussed above, there are 3 types of Automation Cards. You must pick **one** for your Card.
- **Card Key** - Think of this like your Element tag. For example, `hello-world`. This is unique for all Cards and cannot be changed after creation.
- **Requires Identity** - If `true`, when a user installs your Card, they must pick an Identity to give your Card access to. If left unchecked, they won't be prompted and you won't be given access to any of their Identities.

Once you're done defining your Card, continue to create it. Don't worry - only the **Card Key** is permanent - everything else can be changed in the future.

## Scaffolding

Once you successfully create a Card, you will find a new folder in your local App Data that is scaffolded and ready-to-go for your new Card based on the Type you chose. You can find the location of this folder based on your Operating System:

- **Windows** - `C:\Users\{user}\AppData\Roaming\CastHub\automation_cards`
- **macOS** - `TODO:`
- **Linux** - `~/.config/CastHub/automation_cards`

Or by clicking the Folder Button when viewing the Card page.

The folder generated for your Card will be the same as your `key`.

### index.js

All Cards have a single export point. In the provided scaffolded folder, it is `index.js`. The name of the file isn't important, as long as the `main` in your `package.json` points to it.

## Structure

Automation Cards are simple, invisible processes that execute logic internally. Depending on the Type you chose when creating the Card, you will have different methods available to you.

### Triggers

Triggers will traditionally hook in to some system or logic that allows them to listen for data. For example, using a WebSocket, polling an external API, or even something as simple as `setInterval`. Regardless of _how_ they decide they want to trigger, they will all call the same function to trigger the overall Automation to begin execution. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.card.trigger {
    async mounted(): Promise<void> {
        await super.mounted();

        window.setInterval(() => {
            this.trigger(); // This call will begin execution of the Automation.
        }, 5000);
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.trigger {
    async mounted() {
        await super.mounted();

        window.setInterval(() => {
            this.trigger(); // This call will begin execution of the Automation.
        }, 5000);
    }
};
```

</code-block>
</code-group>

This is a very basic example of how Triggers function - more information, including I/O to other Cards, is covered in the [Triggers](/automation-cards/triggers) documentation.

### Conditions

Conditions, whilst not called until an Automation is triggered, are still able to use WebSockets and other asynchronous boot operations before reporting to the App that they are ready. This is done via the `mounted` method.

Along with this, there is an asynchronous function that will be called when the Condition is being asked to resolve or not. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
import { CardIO } from '@casthub/types';

export default class extends window.casthub.card.condition {
    async run(input: CardIO): Promise<void> {
        if (something) {
            throw new Error('You shall not pass!');
        }
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.condition {
    async run(input) {
        if (something) {
            throw new Error('You shall not pass!');
        }
    }
};
```

</code-block>
</code-group>

There isn't a whole lot that goes in to Conditions - but you can read further on how to report failures and prevent an Automation from proceeding in the [Conditions](/automation-cards/conditions) documentation.

### Actions

Much like Conditions, Actions are also able to asynchronously boot before telling the App that it is ready for execution. Also very similar to the Conditions footprint is the method of execution, called when an Automation is triggered and all (if any) Conditions pass. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
import { CardIO } from '@casthub/types';

export default class extends window.casthub.card.action {
    async run(input: CardIO): Promise<void> {
        // Do any business logic here, such as posting a Tweet.
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.action {
    async run(input) {
        // Do any business logic here, such as posting a Tweet.
    }
};
```

</code-block>
</code-group>

Given the wide range of integrated services in the CastHub Ecosystem, you can achieve a lot of interesting and complex operations with Actions. You can read about usage further in the [Actions](/automation-cards/actions) documentation.

## Development

Unpublished Cards will not appear inside the CastHub Store and, as such, you cannot add them to your Automations in the conventional way.

First, make sure you have an Automation ready. It is recommended to have a Automation made purely for Development purposes. Once ready, follow these instructions:

1. Go to the Developers section of the App
2. Click the `Automation Cards` Tab
3. Click the blue "Install" button on your Card

Your local, unpublished Card will now be installed in your Automation.

### Refreshing

If you have Developer Tools enabled, each Card will have an `Inspect` option when you open the dropdown of the individual Card.

Since a Card is loaded in to memory when the App boots, it's somewhat tricky to develop without refreshing the entire App every time you want to see your changes. To combat this, we hook in to refreshes for your Card. This means that when you refresh the Card via Devtools, we load it from the filesystem again and re-inject it in to the Card process.

This can be achieved by refreshing the Devtools window for your Card (`F5` or `[CMD/CTRL] + R`)

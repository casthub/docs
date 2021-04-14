---
title: Triggers
description: Triggers are the starting-point of an Automation, capable of starting the entire Automation lifecycle based on any kind of logic
position: 16
category: Automation Cards
---

Triggers are the starting-point of an Automation, capable of starting the entire Automation lifecycle based on any kind of logic.

## Booting

Triggers are able to asynchronously boot before being ready to execute. This is useful for many things, e.g. fetching external data or establishing WebSocket connections.

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.card.trigger {
    async mounted(): Promise<void> {
        await super.mounted();

        // Establish a WebSocket connection, start long-polling, etc.
    }
};
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.trigger {
    async mounted() {
        await super.mounted();

        // Establish a WebSocket connection, start long-polling, etc.
    }
};
```

</code-block>
</code-group>

## Execution

When a Trigger decides an Automation should run, for example if a new Twitch Subscriber event has come in, it should call the `trigger` function to inform the App that it should begin executing the Automation.

Triggers can also pass an output object down to the Conditions and Actions that will be executed. This is the I/O Object, and has a specific <docs-link path="/automation-cards/io">Schema</docs-link> that must be followed.

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.card.trigger {
    async mounted(): Promise<void> {
        await super.mounted();

        window.setInterval(() => {
            // Must follow the I/O Schema.
            this.trigger({
                meta: {
                    date: (new Date()).getTime(),
                },
            });
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
            // Must follow the I/O Schema.
            this.trigger({
                meta: {
                    date: (new Date()).getTime(),
                },
            });
        }, 5000);
    }
};
```

</code-block>
</code-group>

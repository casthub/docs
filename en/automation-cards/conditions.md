---
title: Conditions
description: Whilst simple, Conditions play a large role in the execution and lifecycle of an Automation
position: 17
category: Automation Cards
---

Whilst simple, Conditions play a large role in the execution and lifecycle of an Automation. Acting as gatekeepers, Conditions decide on whether the Automation should continue and execute any Action(s).

## Booting

Much like [Actions](/automation-cards/actions), they are able to asynchronously boot before being ready to execute.

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.card.trigger {
    async mounted(): Promise<void> {
        await super.mounted();

        // Establish a WebSocket connection, start long-polling, etc.
    }
}
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

When a Trigger is executed, all Conditions will be executed together. In order for the Automation to continue to Actions, all of the Conditions must pass, i.e. not throw any uncaught errors. A function inside the Condition will be called to make this check, `run`. It accepts a single parameter, `input`, which is the data output from the Trigger that executed the Automation.

Each Trigger can define it's own [I/O Schema](/automation-cards/io), as such, it's better to test or hard-encode certain services/systems your Condition works with if you rely on the given `input`.

<code-group>
<code-block label="TypeScript" active>

```typescript
import { CardIO } from '@casthub/types';

export default class extends window.casthub.card.conditions {
    async run(input: CardIO): Promise<void> {
        if (something) {
            /**
             * Throwing an Error is how Conditions report to the
             * App that they have failed
             */
            throw new Error('You shall not pass!');
        }

        /**
         * You don't need to return anything since the function
         * is defined using `async`
         */
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.conditions {
    async run(input) {
        if (something) {
            /**
             * Throwing an Error is how Conditions report to the
             * App that they have failed
             */
            throw new Error('You shall not pass!');
        }

        /**
         * You don't need to return anything since the function
         * is defined using `async`
         */
    }
};
```

</code-block>
</code-group>

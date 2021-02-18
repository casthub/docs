---
title: Actions
description: Actions are the work-house of an Automation. They execute the resulting business logic as set-up by the user
position: 18
category: Automation Cards
---

Actions are the work-house of an Automation. They execute the resulting business logic as set-up by the user.

## Booting

Much like [Actions](/automation-cards/actions), they are able to asynchronously boot before being ready to execute.

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.card.action {
    async mounted(): Promise<void> {
        await super.mounted();

        // Establish a WebSocket connection, start long-polling, etc.
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.action {
    async mounted() {
        await super.mounted();

        // Establish a WebSocket connection, start long-polling, etc.
    }
};
```

</code-block>
</code-group>

## Execution

When a Trigger is executed in an Automation and all Conditions pass, Actions will be executed together (Or asynchronously if the User enables Sequential Actions). A function inside the Action will be called to execute the logic, `run`. It accepts a single parameter, `input`, which is the data output from the Trigger that executed the Automation.

Each Trigger can define it's own [I/O schema](/automation-cards/io), as such, it's better to test or hard-encode certain services/systems your Action works with if you rely on the given `input`.

```js

```
<code-group>
<code-block label="TypeScript" active>

```typescript
import { CardIO } from '@casthub/types';

export default class extends window.casthub.card.action {
    async run(input: CardIO): Promise<void> {
        // Do something!
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.card.action {
    async run(input) {
        // Do something!
    }
};
```

</code-block>
</code-group>

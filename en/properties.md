---
title: Properties
description: Modules and Automation Cards can provide CastHub with a real-time specification of user-defined Properties
position: 23
category: Resources
---

Modules and Automation Cards can provide CastHub with a real-time specification of user-defined Properties. These properties can be modified by the user on-demand inside the CastHub UI itself and your code can react as necessary.

The property specification is defined in your code, and the function itself is asynchronous, allowing you to populate the specification with data fetched from external APIs.

## Providing a Specification

By specifying the method `prepareProps` in your code, you can return a specification that will be used to render the Properties Form to the end-user.

This method is asynchronous, and there is currently no timeout on the length of time this method can take to execute. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
import { PropList, PropType } from '@casthub/types';

export default class extends window.casthub.module<{
    hello: string;
}> {
    async prepareProps(): Promise<PropList> {
        return {
            hello: {
                type: PropType.Text,
                required: true,
                default: 'test',
                label: 'Hello World',
                help: 'Enter any text',
            },
        };
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.module {
    async prepareProps() {
        return {
            hello: {
                type: 'text',
                required: true,
                default: 'test',
                label: 'Hello World',
                help: 'Enter any text',
            },
        };
    }
}
```

</code-block>
</code-group>

For TypeScript users, be sure to provide typings for your properties when writing the class - see above for an example

## Reacting to staged changes

When the User is changing properties in the UI, sometimes you may wish to recalculate the shown Properties based on user input. For example, you could disable a field and wait for another field to be selected first. This is called reacting to **staged changes**, changes which are happening but the user hasn't yet committed to (I.e. hasn't clicked save).

Because of this, it's important to not let these changes affect your code - a best practice would be to keep `prepareProps` stateless.

You can enable this functionality on a per-field basis, since every time that field is changed, the User will be prevented from editing further properties until the `prepareProps` call is executed.

<code-group>
<code-block label="TypeScript" active>

```typescript
import { PropList, PropType } from '@casthub/types';

export default class extends window.casthub.module<{
    select1: string;
    select2: string;
}> {
    async prepareProps(stage): Promise<PropList> {
        return {
            select1: {
                type: PropType.Select,
                required: true,
                label: 'Test 1',
                watch: true,
                options: {
                    option1: {
                        text: 'Option 1',
                    },
                    option2: {
                        text: 'Option 2',
                    },
                },
            },
            select2: {
                type: PropType.Select,
                label: 'Test 2',
                disabled: stage.select1 === undefined,
                options: {
                    anotherOption1: {
                        text: 'Another option 1',
                    },
                    anotherOption2: {
                        text: 'Another option 2',
                    },
                },
            },
        };
    }
}
```

</code-block>
<code-block label="JavaScript">

```js

```

</code-block>
</code-group>

## Reacting to Saved Properties

When a User modifies a property, we provide a way for you to react to this change in real-time through another method, `onPropChange`. This method is also called for every property on initial load.

<code-group>
<code-block label="TypeScript" active>

```typescript
onPropChange(key: string, value: any, initial: boolean): void {
    //
}
```

</code-block>
<code-block label="JavaScript">

```js
onPropChange(key, value, initial) {
    //
}
```

</code-block>
</code-group>

## Supported Types

Every field in the property specification must provide a `type`. This defines how the property will be rendered to the user, and each has different additional fields.

- `text` - Renders a textfield for the user to freely enter any text.
- `toggle` - Renders a switch-like component that can be toggled on/off.
- `slider` - Renders a slider that forces an integer in the given range.
    - Requires the `range` key, which is an Array of 2 Integers defining the min/max range. E.g. `range: [1, 10]`. The `default` of this field must be within this range.
- `select` - Renders a select/dropdown for the user to pick one of the given options.
    - Requires the `options` key, which is an Object of key-object option pairs. E.g.
        ```js
        options: {
            twitch: {
                text: 'Twitch',
                icon: 'twitch',
            },
        },
        ```
      The `default` of this field should link to one of the keys for an option.

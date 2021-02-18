---
title: TypeScript
description: CastHub provides first-class support to TypeScript users
position: 24
category: Resources
---

CastHub provides first-class support to TypeScript users. We have a full-coverage typings package that we maintain and update.

<code-group>
<code-block label="Yarn" active>

```bash
yarn add @casthub/types --dev
```

</code-block>
<code-block label="NPM">

```bash
npm install @casthub/types --dev
```

</code-block>
</code-group>

Once installed, you can register the typings manually in your `tsconfig.json`. For example:

```json
{
    "compilerOptions": {
        ...
        "types": [
            ...
            "@casthub/types"
        ]
        ...
    },
    ...
}
```

And that's it! You can explore the typings yourself, or browse through our Documentation for various examples written in TypeScript.

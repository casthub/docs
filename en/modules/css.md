---
title: CSS
description: There are a few different methods of supplying CSS to your Module
position: 8
category: Modules
---

There are a few different methods of supplying CSS to your Module. You are free to choose whichever one works best for you.

## Template File

Modules can supply a Template HTML File which can include CSS inside of it. This is the easiest way to include and manage CSS in a Module. You can <docs-link path="/modules/html">read the Documentation here</docs-link>.

## Embedded in JavaScript

A lower-level method of adding CSS to your Module is via the `css` property within your Module. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.module {
    constructor() {
        super();

        this.css = `
            .module {
                display: flex;
            }
        `;
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.module {
    constructor() {
        super();

        this.css = `
            .module {
                display: flex;
            }
        `;
    }
};
```

</code-block>
</code-group>

## Building & Pre-processors

A combination of a build process and the previous in-module CSS, you can use a build process like Rollup to export a Module that pre-imports the required CSS/SCSS/LESS file. For more information, check out our <docs-link path="/modules#advanced-boilerplate">Boilerplate Documentation</docs-link>.

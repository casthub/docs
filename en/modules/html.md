---
title: HTML
description: CastHub Modules can specify a HTML Template
position: 7
category: Modules
---

CastHub Modules can specify a HTML Template file in their `package.json` which will be injected in to the Module and available at runtime. This is identical to a `<template>` for [re-usable HTML](https://developer.mozilla.org/en-US/Web/HTML/Element/template), and as such, the use of `<slot>` is allowed.

You can specify a relative URL to your Template file in your Module `package.json`:

```json
{
    "casthub": {
        "files": {
            "template": "src/template.html"
        }
    }
}
```

This HTML file can include any HTML you'd like for your Element. It won't be rendered on-page (Since it is wrapped in a `<template>`), but will be available to your Module instantly using the `this.template()` function. For example:

<code-group>
<code-block label="TypeScript" active>

```typescript
export default class extends window.casthub.module {
    constructor() {
        super();

        // Clone a new HTML Node from the Template.
        const $tmpl = this.template();

        // $tmpl is now a fresh clone of your Template file.
    }
}
```

</code-block>
<code-block label="JavaScript">

```js
module.exports = class extends window.casthub.module {
    constructor() {
        super();

        // Clone a new HTML Node from the Template.
        const $tmpl = this.template();

        // $tmpl is now a fresh clone of your Template file.
    }
};
```

</code-block>
</code-group>

Since the Template File can be directly appended to the Module Container, you can also specify your CSS in there:

```html
<style type="text/css">
    .hello-world {
        color: #FF0000;
    }
</style>
<p class="hello-world">Hello World!</p>
```

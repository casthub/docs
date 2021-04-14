---
title: Components
description: ''
position: 13
category: Modules
---

We expose some Web Components to Modules directly from the CastHub App itself. By doing this, we aim to aid in Module prototyping, as well as allowing Module Developers to create Modules that are in-line with the App Design and branding if they choose to do so.

Whilst these Components are _technically_ Web Components, we still recommend using the `window.casthub.create()` helper to create them. There are some caveats to using our Web Components inside of Modules which using this helper solves by applying patches automatically.

## `button`

**Properties**

| Name            | Type    | Default     | Values                                             |
|-----------------|---------|-------------|----------------------------------------------------|
| `color`         | String  | `'primary'` | `['primary', 'secondary', 'success', 'danger']`    |
| `icon`          | String  | `''`        | <docs-link path="/icons">Icon Explorer</docs-link> |
| `icon-position` | String  | `'left'`    | `['left', 'right']`                                |
| `flat`          | Boolean | `false`     |                                                    |
| `floating`      | Boolean | `false`     |                                                    |
| `small`         | Boolean | `false`     |                                                    |
| `disabled`      | Boolean | `false`     |                                                    |

**Events**

The Button Component only emits a single event, which is `click`. Given Components are destroyed when the Module is, you don't need to worry about removing any listeners.

```js
const $button = window.casthub.create('button');
$button.addEventListener('click', () => {
    console.log('Hello world');
});
```

**Examples**

```js
const $button = window.casthub.create('button');
$button.color = 'primary';
$button.icon = 'casthub';
$button.iconPosition = 'right';
$button.innerText = 'Hello World';

// Add the Button to the DOM.
this.addEl($button);
```

```js
// Create a new Element.
this.$newElement = document.createElement('div');
this.addEl($newElement);

const $button = window.casthub.create('button');
$button.color = 'danger';
$button.icon = 'close';
$button.innerText = 'Don\'t Press';
$button.addEventListener('click', () => {
    alert('You shouldn\'t have done that...');
});

// Append the Button to an existing Element.
this.$newElement.appendChild($button);
```

```js
const $button = window.casthub.create('button');
$button.color = 'secondary';
$button.floating = 'true';
$button.small = 'true';
$button.icon = 'check';

// Add the Button to the DOM.
this.addEl($button);
```

## `empty`

The Empty Component is a simple empty indicator that can be used via the `empty` Component.

**Properties**

| Name      | Type    | Default                    | Values                                             |
|-----------|---------|----------------------------|----------------------------------------------------|
| `icon`    | String  | `''`                       | <docs-link path="/icons">Icon Explorer</docs-link> |
| `text`    | String  | `'There\'s nothing here!'` |                                                    |
| `overlay` | Boolean | `false`                    |                                                    |
| `visible` | Boolean | `true`                     |                                                    |

**Examples**

```js
const $empty = window.casthub.create('empty');
```

```js
const $empty = window.casthub.create('empty');
$empty.icon = 'twitch';
$empty.text = 'Hello World';
```

## `header`

The Header Component is a stylized, pre-made Module Header that can be used via the `header` Component.

**Properties**

| Name    | Type   | Default     | Values                                             |
|---------|--------|-------------|----------------------------------------------------|
| `icon`  | String | `''`        | <docs-link path="/icons">Icon Explorer</docs-link> |
| `color` | String | `'#000000'` |                                                    |

**Examples**

```js
$header = window.casthub.create('header');
$header.icon = 'streamlabs';
$header.color = '#31c3a2';
$header.innerText = 'Top Donations';
```

```js
$header = window.casthub.create('header');
$header.icon = 'twitch';
$header.color = '#4b367c';
$header.innerText = 'Latest Subscribers';
```

## `icon`

The Icon Component renders a simple, inline Icon. You can choose from any of the <docs-link path="/icons">available icons</docs-link>.

**Examples**

```js
const $icon = window.casthub.create('icon', 'casthub');
this.$container.appendChild($icon);
```

If you need to change the type of Icon being displayed, you can just change the `type` attribute of the element:

```js
const $icon = window.casthub.create('icon', 'casthub');
$icon.setAttribute('type', 'twitch');
```

## `select`

The Select Component closely follows Material Design specifications and has two different styles - `default` and `outlined`.

**Properties**

| Name                            | Type    | Default | Values |
|---------------------------------|---------|---------|--------|
| `id` <badge>Required</badge>    | String  |         |        |
| `items` <badge>Required</badge> | Array   |         |        |
| `title` <badge>Required</badge> | String  |         |        |
| `value`                         | String  | `''`    |        |
| `outlined`                      | Boolean | `false` |        |
| `error`                         | String  | `''`    |        |
| `help`                          | String  | `''`    |        |
| `disabled`                      | Boolean | `false` |        |
| `filter`                        | Boolean | `false` |        |
| `visible`                       | Number  | `-1`    |        |

**Examples**

```js
const $select = window.casthub.create('select');
$select.id = 'hello-world-1';
$select.title = 'Hello world!';
$select.help = 'Just a simple Select';
$select.items = [
    {
        text: 'Test 1',
        value: 'test-1',
    },
    {
        text: 'Test 2',
        value: 'test-2',
    },
    {
        text: 'Test 3',
        value: 'test-3',
    },
];
```

```js
const $select = window.casthub.create('select');
$select.id = 'hello-world-2';
$select.title = 'Hello world 2!';
$select.outlined = true;
$select.filter = true;
$select.visible = 2;
$select.items = [
    {
        text: 'Test 1',
        value: 'test-1',
    },
    {
        text: 'Test 2',
        value: 'test-2',
    },
    {
        text: 'Test 3',
        value: 'test-3',
    },
];
```

## `textfield`

The Textfield Component closely follows Material Design specifications and has two different styles - `default` and `outlined`.

**Properties**

| Name                            | Type    | Default | Values |
|---------------------------------|---------|---------|--------|
| `id` <badge>Required</badge>    | String  |         |        |
| `title` <badge>Required</badge> | String  |         |        |
| `value`                         | String  | `''`    |        |
| `outlined`                      | Boolean | `false` |        |
| `error`                         | String  | `''`    |        |
| `help`                          | String  | `''`    |        |
| `max`                           | Number  | `-1`    |        |
| `disabled`                      | Boolean | `false` |        |

**Examples**

```js
const $textfield = window.casthub.create('textfield');
$textfield.id = 'hello-world-1';
$textfield.title = 'Hello world!';
$textfield.help = 'Just a simple Textfield';
```

```js
const $textfield = window.casthub.create('textfield');
$textfield.id = 'hello-world-2';
$textfield.title = 'Hello world!';
$textfield.outlined = true;
$textfield.max = 20;
$textfield.error = 'Whoops!';
```

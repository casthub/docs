---
title: Storage
description: Modules do not have access to conventional persistent storage methods, such as localStorage
position: 12
category: Modules
---

Modules do not have access to conventional persistent storage methods, such as `localStorage`. Instead, a Storage API is exposed to Modules that allows different transports for storage:

- `local` - Stores data on the User's local Computer
- `cloud` - Stores data in the CastHub Cloud for use anywhere

## Storing Data

```js
await this.store.set({
    key: 'hello-world',
    val: 'Testing 123!',
    transport: 'local', // Optional: Defaults to `local`
});
```

## Fetching Data

```js
const val = await this.store.get({
    key: 'hello-world',
    def: 'Value not set', // Optional: A default value if none exists
    transport: 'local', // Optional: Defaults to `local`
});
```

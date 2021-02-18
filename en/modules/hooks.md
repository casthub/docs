---
title: Lifecycle Hooks
description: Modules can use hooks to execute logic at given times during the lifecycle of a Module
position: 9
category: Modules
---

Modules can use hooks to execute logic at given times during the lifecycle of a Module. These are all called as functions within the Module instance itself.

## `created`

Like `mounted`, this is called as soon as the Module is instantiated. It will always be called before `mounted` and allows any bootstrapping of includes/other set-up. It is **synchronous**.

## `mounted`

The most basic hook, this is called when, as the name suggests, the Module Element is **mounted** to the DOM. It is asynchronous, and you should return a `Promise` from it. Whilst resolving, a loader will be overlayed on to the Module Element. This is useful for fetching initial data.

If the returned Promise is rejected, the message given to `reject` will be shown to the User as feedback.

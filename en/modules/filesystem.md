---
title: Filesystem
description: Modules are allowed limited access to store data on the filesystem
position: 11
category: Modules
---

Modules are allowed limited access to store data on the filesystem. This is useful for exporting information that can easily be digested by other Applications, such as your current Spotify song for use in broadcasting software.

You are limited to the following extensions/data types:

- `json`
- `md`
- `txt`

## File Specification

Usage of the Filesystem API is gated by a register of files you wish to use, meaning you cannot create/delete/update files you do not own. To achieve this, you can use the <docs-link path="/modules/metadata">metadata</docs-link> specification to define which files you'd like to use inside your `package.json`. For example:

```json
{
    "casthub": {
        "filesystem": {
            "test": {
                "filename": "test",
                "extension": "md"
            },
            "anotherTest": {
                "filename": "hello",
                "extension": "json"
            }
        }
    }
}
```

CastHub will ensure that the correct folder exists for your Module and that the specified files exist before the Module is loaded.

Since providing default content for files is not allowed, any `json` files will default to an empty object, allowing the retrieval of the file without error.

## Locating Files

For use-cases like exporting data for external Applications to consume, it's important to make it as easy as possible for the User to find the files you generate. As such, we include an `Open Folder` option when hovering over a Module if it has any files.

Otherwise, the structure for files is as follows:

```
%CASTHUB%/module_files/{key}/{module_id}
```

- `%CASTHUB%` - The <docs-link path="/modules#template-scaffolding">appdata location</docs-link> for CastHub
- `{key}` - The key of the Module, e.g. `twitch-user`
- `{module_id}` - The UUID for this instance of the Module

## Storing Data

We offer a simple API for storing data in given Files.

You do not specify files by their path, but instead **by the key** you assign in your `package.json`.

For example, using the Filesystem specification above, we can do the following:

```js
await this.filesystem.set('test', `# Hello world!`);
await this.filesystem.set('anotherTest', {
    something: 'Hello world!',
});
```

As you can see, `json` files accept an `Object`/`Array`, which will be encoded for you. Otherwise, data **will be cast to a string**.

## Retrieving Data

Just as easily, you can retrieve data from files using a very similar API.

```js
const test = await this.filesystem.get('test');
const anotherTest = await this.filesystem.get('anotherTest');
```

`json` files will be decoded when retrieved, whereas other extensions **will return a string**.

## Limitations

- You can only have up to **10** file specifications at any given time.
- `filename` in the `package.json` can be up to **32 characters in length**. If larger, it will be shortened.
    - Additionally, `filename` must be **alphanumeric**, including underscores `_` and dashes `-`. All other characters will be stripped out.
- Any files that you remove from your `package.json` **will be deleted** when the User downloads the update.

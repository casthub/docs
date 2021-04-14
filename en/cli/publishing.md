---
title: Publishing
description: ''
position: 5
category: CLI
---

Publishing to the CastHub Store is a very short and easy process. Through the in-app **Developers** section, you'll have access to all owned/managed Modules and Automation Cards. You can find the Developers section by clicking your avatar in the titlebar of the App.

Through the Developers section of the App, you can create new content ready for use in the CastHub CLI.

## Topics

The CastHub CLI supports all content types offered by CastHub - since all of these generally follow the same format, commands are broken down in to an almost identical topic-based format. When a `[topic]` is mentioned in the CLI Documentation, it will refer to the type of content you are uploading/publishing, namely:

- `module`
- `automation-card`

## Uploading Code

Using the CastHub CLI, you can upload and/or publish your content via your Terminal or <docs-link path="/cli/ci">CI/CD</docs-link>. Make sure you have <docs-link path="/cli">installed and set-up the CLI</docs-link>!

In the directory of your content, you can call the `upload` command of the relevant topic to upload it to the CastHub CDN:

```sh
$ casthub [topic]:upload
```

This will attach the code to the version specified in `package.json`, unless that version is already published. If the version does not exist, the CLI will attempt to create it for you.

There are various flags available to the `upload` command - all of which you can find via the `casthub help [topic]:upload` command. However, some flags worth noting will be documented below.

### Automatic Publishing

The `-p/--publish` flag will automatically publish the given Version when added. This is useful for <docs-link path="/cli/ci">CI/CD</docs-link> environments where it is not possible to launch the App to manually publish versions after upload.

## Initial Version

From the get-go, your content will have a `v1.0.0` Version attached to it, and you will be required to publish this before being able to create new Versions.

Once this Version is published, your content will also be published and listed on the Store.

## Updates

Publishing updates uses the same format as above - just increment the `version` in `package.json` to your liking, and call the usual CLI commands to upload/publish.

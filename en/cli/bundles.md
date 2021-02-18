---
title: Bundles
description: CastHub CLI will bundle your files in a manner similar to an NPM module
position: 3
category: CLI
---

CastHub CLI will bundle your files in a manner similar to an NPM module (In fact, most of the core libraries used are the same as NPM). However, the CLI will ignore `.npmignore` and `.gitignore` definitions, and only bundle the files needed by the specification defined in your `package.json`.

This is because CastHub is very strict on file formats and inclusion - for the most part, this is limited to bundling your `package.json` `main`, as well as any `template` definitions. You can view the documentation for these to learn more about their purpose:

- [Module HTML Template](/modules/html)

CastHub does not support binary imports, such as images or audio, as of right now.

## Closed-source

Since CastHub CLI will only bundle files **required** by the metadata specification, it is very easy to produce content that is closed-source.

An example of this is our [advanced Module Boilerplate](/modules#advanced-boilerplate), which uses compilation to generate a library file for upload. Since none of the source material is needed by CastHub, none of it will be bundled by the CLI.

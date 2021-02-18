---
title: Metadata
description: Module Metadata is defined in the package.json of your Module
position: 10
category: Modules
---

Module Metadata is defined in the `package.json` of your Module. Generally it follows the standard definition for a `package.json`, as shown below. However there are some CastHub-specific metadata options, defined in a `casthub` object within the JSON.

- `name` - This should be the same as your **Module Key**
- `version` - This should match a version you have created for the Module in the Management Interface
- `main` - Must point to your Module Web Component export
- `casthub` - An object containing CastHub-specific metadata
    - `width` - The initial width for your Module
    - `height` - The initial height for your Module
    - `min_width` - [Optional] The minimum width for your Module
    - `min_height` - [Optional] The minimum height for your Module
    - `max_width` - [Optional] The maximum width for your Module
    - `max_height` - [Optional] The maximum height for your Module
    -  `files` - [Optional]
        - `template` - The relative path to the Template HTML file

Any fields not specified above are not required and will be ignored.

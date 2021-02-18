---
title: Stock Elements
description: ''
position: 14
category: Modules
---

Modules have access to some pre-made Stock Elements maintained by CastHub. This helps to keep the general theme of CastHub aligned as well as help speed-up prototyping.

All Stock Elements are instances of `window.casthub.module` and as such can be extended as the main export of a Module. They all live within the `window.casthub.elements` namespace, for example, `window.casthub.elements.user`.

## Chat

You can use this Stock Element by extending the exported global: `window.casthub.elements.chat`

This Stock Element will render a ready-to-go Chat System with an easy-to-use Message management system.

### Adding a Message

Messages must abide by a given specification to be rendered in the Module. See below for the object specification.

All Messages added to the Stock Element require a unique ID. This will avoid duplicate messages being added.

```js
this.add('some-unique-id', {
    sender: {
        name: 'John Smith', // The name of the Sender.
        color: '#FF0000', // Optional. Used to color the Name of the Sender.
        badges: [], // Optional, ordered array of image URLs for Badges.
    },
    /**
     * The text content of the Message.
     * This supports HTML for things like emoticons.
     */
    message: 'Hello <strong>World</strong>',
});
```

### Removing a Message

Given all Messages require a unique identifier, it's relatively easy to remove an existing Message from the UI.

```js
this.remove('some-unique-id');
```

## Counter

You can use this Stock Element by extending the exported global: `window.casthub.elements.counter`

The Counter Stock Element renders a single Counter that comes with a background Chart to represent change in the count value over time. You can see it in-use in any of our <a href="https://github.com/casthub/module-twitch-viewers" target="_blank" rel="noopener noreferrer">Viewer Count Modules</a>.

### Setting the Count value

Setting the count value is as easy as assigning the `count` property of the Module to an integer. This will also add it as a data point for the background Chart, automatically updating and animating it to reflect the change. For example:

```js
this.count = 5;

/**
 * Whilst the count will immediately reflect the following value,
 * the previous value will still be added as a data point for the
 * Chart.
 */
this.count = 10;
```

### Customization

Currently the Stock Element is limited in customizability to the following properties:

- `color` - The color used for the background Chart. Can be `rgb(a)` or `hex`.
- `max` - An integer value of the maximum amount of data points the Chart can have.

These properties **are reactive** and can be updated at any point during the module lifecycle.

## Emote Counter

You can use this Stock Element by extending the exported global: `window.casthub.elements.emote_counter`

This Stock Element will render an extendable list of Emote instances with counts, and can automatically sort based on the most popular Emotes.

### Adding an Emote

Emotes must be registered before they can have their counts incremented.

This just requires the unique Emote ID, as well as an Image that represents the Emote.

```js
this.init('some-unique-id', 'https://example.com/test.png');
```

### Incrementing an Emote Count

Using the same Emote ID an Emote was registered with, you can increment the count via the following method:

```js
this.inc('some-unique-id');
```

It is also possible to pass a second, integer-based parameter to increment the count by more than `1`. For example:

```js
this.inc('some-unique-id', 5); // Adds 5 to the count instead of 1
```

### Auto-sorting

By default, Emotes will be sorted based on the counts they have. You can disable this by setting `this.shouldSort` to `false` in your Modules' `constructor`:

```js
constructor() {
    super();

    // Disable automatic sorting.
    this.shouldSort = false;
}
```

## Feed

You can use this Stock Element by extending the exported global: `window.casthub.elements.feed`

The Feed Stock Element renders a scrollable live-feed for easy consumption of time-based events. For example, we use the Feed Stock Element in the various Service Feeds, such as the <a href="https://github.com/casthub/module-twitch-feed" target="_blank" rel="noopener noreferrer">Twitch Feed Module</a>.

### Adding or Removing Items

You can add or remove Items from the List using the `add` and `remove` methods. The signature for these methods is super-simple:

- `add(id, item)` - Items **must** have a unique `id`!
- `remove(id)` - Removes an Item based on the unique `id`.

### Item Definition

When adding an Item object, it must follow a given definition.

```js
{
    // All Items are required to have a text body.
    text: 'Anonymous just subscribed!',
    // One of the Icons supported by CastHub.
    icon: 'twitch',
    // Icon can also be an Object if you want to define a color.
    icon: {
        type: 'twitch',
        color: '#FF0000',
    },
    /**
     * By default, the date will be relative to the time the Item was added.
     * However, you can specify your own millisecond UNIX timestamp:
     */
    date: (new Date()).getTime(),
}
```

## List

You can use this Stock Element by extending the exported global: `window.casthub.elements.list`

The List Stock Element is a great base element for any, well, lists. For example, we use the List Stock Element in the various Donation Modules, such as the <a href="https://github.com/casthub/module-tipeeestream-donations" target="_blank" rel="noopener noreferrer">Tipeeestream Donations Module</a>.

### Adding or Removing Items

You can add or remove Items from the List using the `add` and `remove` methods. The signature for these methods is super-simple:

- `add(id, item)` - Items **must** have a unique `id`!
- `remove(id)` - Removes an Item based on the unique `id`.
- `clear()` - Removes **all** Items from the list.

If you'd like to manually access the current list items, you can use the `this.items` Object.

### Item Definition

When adding an Item object, it must follow a given definition.

```js
{
    // All Items are required to have a title.
    title: 'My Item',
    // Subtext displays below the title.
    subtext: 'Hello World',
    // Avatar must be a URL to an image if they're enabled.
    avatar: '',
    // An optional `update` function that is run once a second.
    update(item) {
        // `item` is a reference to the item data that you can update.
    },
}
```

### Limiting List Length

To keep your List to a specific length, you can set the `limit` property in your `constructor`. For example:

```js
constructor() {
    super();

    this.limit = 5;
}
```

This property is reactive and if lessened, will remove any overflowing items. However, if increasing the value, it will not show more items until new ones are added.

By default, `limit` is set to `-1`, which means the list will not limit the Items you add.

### Avatars

Avatars are disabled for List Items by default. You can enable them by setting the `avatars` property to `true` in your `constructor`. For example:

```js
constructor() {
    super();

    this.avatars = true;
}
```

### Sorting and Filtering

By implementing the optional functions, you can easily filter and sort any of the List Items that you add automatically.

```js
/**
 * Filters out any unwanted Items.
 *
 * @param {Array} items
 *
 * @return {Array}
 */
filter(items) {
    return items;
}

/**
 * Sorts the given List Items.
 *
 * @param {Array} items
 *
 * @return {Array}
 */
sort(items) {
    return items;
}
```

### Modifying the Header

The List Stock Element comes with a [Header Component](/modules/components#header) and provides short and easy setters for modifying it. For example:

```js
constructor() {
    super();

    this.icon = 'tipeeestream';
    this.title = 'Latest Donations';
    this.color = '#313747';
}
```

## Player

You can use this Stock Element by extending the exported global: `window.casthub.elements.player`

The Media Player Element, by default, gives you the following scaffolding:

- Previous, Play/Pause and Next buttons
- Album Cover Art
- Song Name
- Artist(s)

### Control Methods

In order to make the media controls work, you must implement functions for each, all of them returning a `Promise`. For example, this is from the <a href="https://github.com/casthub/module-spotify-player" target="_blank" rel="noopener noreferrer">Spotify Player</a> Module:

```js
/**
 * Called to request to play the Media source.
 *
 * @return {Promise}
 */
async play() {
    await this.fetch({
        method: 'PUT',
        url: 'me/player/play',
    });
}
```

With methods that also run the logic for `previous`, `pause` and `next` all looking the same. These will be called when the User presses the associated button.

### Name and Artist

The Name of the Media can be set via `this.setTitle('Hello world!');`.

Setting the Artist is the same, `this.setArtist('Hello world!');`, but if the Media has multiple Artists, you can pass through an `Array` of names, then we'll handle the rest. For example:

```js
this.setArtist([
    'Name 1',
    'Name 2',
]);
```

### Album Image Art

If Album Art is present, you can set it via the `this.setAlbumImage(url);` method. This will pre-load the Image before setting it in the UI, preventing any flickering as the image loads.

If you want to remove the Album Art, call `this.setAlbumImage(null);`

### Disabled State

Some Media sources, such as Spotify, will not return any data when the Player is not open on the Users end, and none of the controls will function. In times like this, it is beneficial to disable the controls and re-enable them once functionality returns.

This can be achieved by called `this.setDisabled(true);`, and using `false` to re-enable the controls.

## Scenes

You can use this Stock Element by extending the exported global: `window.casthub.elements.scenes`

The Scenes Element displays a grid of Scenes that can be switched to/from. It is used in a few of our Modules, including the <a href="https://github.com/casthub/module-obs-scenes" target="_blank" rel="noopener noreferrer">OBS Scenes</a> Module.

### Setting and Adding Scenes

Adding Scenes is relatively easy. You have two options: `add` or `set`. Whilst similar, there are some differences:

- `set` takes a single `Object` parameter using the `id => sceneObject` structure. It will also **remove** and existing Scenes that are not in the Object.
- `add` takes two parameters, `id` and `sceneObject`. It doesn't affect other Scenes and will just add the specified Scene.

The following example shows the usage of both:

```js
this.add('test-1', {
    title: 'Scene 1',
});

// Since this doesn't include `test-1`, it will be removed.
this.set({
    'test-2': {
        title: 'Scene 2',
    },
    'test-3': {
        title: 'Scene 3',
    },
});
```

### Removing Scenes

Removing a Scene is pretty straightforward:

```js
this.add('test-1', {
    title: 'Scene 1',
});

this.remove('test-1');
```

### Responding to Scene changes

Once the User has selected a new Scene, it's up to the Module to decide how to work with that. You can override the `setScene` method to implement this logic.

```js
setScene(id) {
    // The User has selected the Scene with ID `id`.
}
```

### Setting the Active Scene

Since all software responds to changes differently, you'll have to manually set the currently active Scene. For example, you could set it immediately after the User sets it, or wait for an asynchronous Event before setting it.

```js
this.setActive('scene-id');
```

## Stream Status

You can use this Stock Element by extending the exported global: `window.casthub.elements.stream_status`

The Stream Status Element can display a simple OFFLINE/LIVE indicator with a bit of customizability. It is used in a few of our Modules, including the <a href="https://github.com/casthub/module-twitch-stream-status" target="_blank" rel="noopener noreferrer">Twitch Stream Status</a> Module.

### Toggling the Live Status

Using the `live` property within the Module, you can easily toggle the live status by setting the property to `true/false`. The Module UI will react accordingly.

### Customization

Currently the Stock Element is limited in customizability to the following properties:

- `icon` - The Icon places in the top-left of the Module. A list of supported Icons can be found [here](/icons).

These properties **are reactive** and can be updated at any point during the module lifecycle.

## User

You can use this Stock Element by extending the exported global: `window.casthub.elements.user`

The User Info Stock Element provides a base for presenting labeled information to the User, such as Stream Online status and Follower Count. An example of where this is used would the the many different User Modules, like the <a href="https://github.com/casthub/module-twitch-user" target="_blank" rel="noopener noreferrer">Twitch User Module</a>.

### Labels

The most powerful part of the User Stock Element is the Labels system. This allows you to display labeled short text or counts to the User and update them easily.

To make use of this system, you have to define your List items in your `constructor`, for example:

```js
this.setLabels({
    followers: {
        name: 'Followers',
        label: -1,
        enabled: true,
    },
    subscribers: {
        name: 'Subscribers',
        label: -1,
        enabled: false,
    },
});
```

This creates two Labels, **Followers** and **Subscribers**. Let's break-down the object:

- `name` - The text displayed above the contents of the Label.
- `label` - The initial content for the Label. Labels can be `Number` or `String`.
    - If a Label is a `Number` and is initially `-1`, it will be displayed as a `-` character.
    - If a `Number`, it will also be formatted as a number.
- `enabled` - Whether the Label should be showed by default. You can enable/disable Labels whenever you like, this is just the initial state.

Now that you have registered your Label definitions, you can update them at any point in time with the following methods:

- `this.setLabel(key, value)` - Set the `key` Label to the given `value`. E.g.:
    - `this.setLabel('followers', 5)`
    - `this.setLabel('online', 'OFFLINE')`
- `this.setLabelEnabled(key, enabled)` - Sets whether the `key` Label is `enabled` or not. E.g.:
    - `this.setLabelEnabled('subscribers', true)`

### Other Customization

The following properties can be set to further customize the Stock Element:

- `avatar` - Sets the Avatar Image to the given URL.
- `color` - Sets the color of the header block.
- `icon` - Sets an Icon shown to the left of the header block, see a list of available Icons [here](/icons).

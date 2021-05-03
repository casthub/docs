---
title: Third-Party
description: One of the many benefits from the CastHub Ecosystem is access to a diverse range of third-party integrations
position: 25
category: Resources
---

One of the many benefits from the CastHub Ecosystem is access to a diverse range of third-party integrations. Currently these come in the forms of unified API interfaces, WebSocket management and access to local applications.

## Discord

We support the latest available Discord HTTP API through CastHub. **Currently this is `v9`**. For example:

```js
const response = await window.casthub.fetch({
    integration: 'discord',
    method: 'GET',
    url: 'users/@me/guilds',
});
```

All requests are authenticated under the User, we do not currently support using a Bot account.

## Elgato Control Center

CastHub makes controlling Elgato Control Center easy by communicating on your behalf. For example:

```js
async mounted() {
    const { id } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    // Listen to specific Control Center events.
    ws.on('deviceAdded', data => {
        console.log('deviceAdded', data);
    });

    // Request a specific command response.
    const response = await ws.send('getApplicationInfo');
    console.log(response);

    await super.mounted();
}
```

## Instagram

Code requiring an integration for Instagram are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the Username and Media Count from Instagram:

```js
const response = await window.casthub.fetch({
    integration: 'instagram',
    method: 'GET',
    url: 'me',
    query: {
        fields: [
            'id',
            'username',
            'media_count',
        ].join(','),
    },
});
```

This supports the GraphQL options from the **Facebook** <a href="https://developers.facebook.com/instagram-api/reference" target="_blank" rel="noopener noreferer">Instagram API</a>. **Yes, it's a big step down from the old Instagram API,** however this is the best we can do now that the old Instagram API is closed.

## Nightbot

Code requiring an integration for Nightbot are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting all Custom Commands from Nightbot:

```js
const response = await window.casthub.fetch({
    integration: 'nightbot',
    method: 'GET',
    url: 'commands',
});
```

This supports the REST API from <a href="https://api-docs.nightbot.tv/#introduction" target="_blank" rel="noopener noreferer">Nightbot</a>.

## OBS Studio

CastHub offers an managed communication method between the Users' computer and OBS Studio. For example:

```js
async mounted() {
    await super.mounted();

    const { id } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    // Listen to stock events.
    ws.on('ScenesChanged', async () => {
        console.log('Scenes have changed');
    });

    // We can also emit events to change OBS.
    ws.send('SetCurrentScene', {
        'scene-name': 'test-1',
    });
}
```

If a connection to that specific Identity is already open (For example, when another Module/Automation has already opened it), you will be subscribed to events from that instance straight away and won't need to wait for a new connection to open.

You can find the reference material for <a href="https://github.com/Palakis/obs-websocket/blob/4.x-current/docs/generated/protocol.md" target="_blank" rel="noopener noreferer">supported Events and Commands here</a>.

## Patreon

Code requiring an integration for Patreon are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting all Campaigns from Patreon:

```js
const response = await window.casthub.fetch({
    integration: 'patreon',
    method: 'GET',
    url: 'campaigns',
    data: {
        'fields[campaign]': [
            'creation_name',
            'image_url',
            'patron_count',
        ].join(','),
    },
});
```

This supports the <a href="https:/.patreon.com/#apiv2-resource-endpoints" target="_blank" rel="noopener noreferer">Patreon v2 API</a>.

## Spotify

Code requiring an integration for Spotify are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the current song from Spotify:

```js
const response = await window.casthub.fetch({
    integration: 'spotify',
    method: 'GET',
    url: 'me/player',
});
```

This supports the <a href="https://developer.spotify.com/documentation/web-api/reference-beta/" target="_blank" rel="noopener noreferer">Spotify API reference</a>.

## StreamElements

Code requiring an integration for StreamElements are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the latest 5 Tips from StreamElements:

```js
const { identity } = this.identity;

const response = await window.casthub.fetch({
    integration: 'streamelements',
    method: 'GET',
    url: `tips/${identity}`,
    data: {
        limit: 5,
        sort: 'createdAt',
    },
});
```

This supports the <a href="https:/.streamelements.com/reference/giveaways" target="_blank" rel="noopener noreferer">StreamElements v2.0 API reference</a>.

## Streamlabs

### API

Code requiring an integration for Streamlabs are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the latest 5 Tips from Streamlabs:

```js
const response = await window.casthub.fetch({
    integration: 'streamlabs',
    method: 'GET',
    url: 'donations',
    data: {
        limit: 5,
    },
});
```

This supports the <a href="https://dev.streamlabs.com/listing-donations" target="_blank" rel="noopener noreferer">Streamlabs REST API reference</a>.

### WebSocket

CastHub makes using Streamlabs WebSockets easy by managing all identities for you. For example:

```js
async mounted() {
    await super.mounted();

    const { id } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    // And register the new Tip Event.
    ws.on('donations', data => {
        console.log(data);
    });
}
```

If a connection to that specific Identity is already open (For example, when another Module/Automation has already opened it), you will be subscribed to events from that instance straight away and won't need to wait for a new connection to open.

## Streamlabs OBS

You can connect to a locally hosted Streamlabs OBS instance using the familiar CastHub WS system. For example:

```js
const { id } = this.identity;
const ws = await window.casthub.ws(id);

ws.on('ScenesService.sceneAdded', (data) => {
    console.log('A scene was added:', data)
});

const currentScene = await ws.send('activeSceneId', {
    resource: 'ScenesService',
});

console.log('The active scene is:', currentScene);
```

Streamlabs OBS supplies various Services which expose Methods for you to use. When making a request to SLOBS, you specify the service in the request data as `resource`, whereas for listening to events, you prefix the event name with the service. For example, `ScenesService.sceneAdded`.

CastHub will automatically subscribe you to any Events you listen for, so you don't have to send the subscription request yourself.

You can find a list of all supported Services and Methods via the [Streamlabs OBS Documentation](https://stream-labs.github.io/streamlabs-obs-api-docs/docs/index.html).

## Tipeestream

Code requiring an integration for Tipeestream are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the latest 20 Donations from Tipeestream:

```js
const response = await window.casthub.fetch({
    integration: 'tipeeestream',
    method: 'GET',
    url: 'events.json',
    data: {
        type: ['donation'],
        limit: 20,
        sort: 'createdAt',
        order: 'desc',
    },
});
```

This supports the <a href="https://api.tipeeestream.com/api-doc/" target="_blank" rel="noopener noreferer">Tipeestream API reference</a>.

## Twitch

### API

Code requiring an integration for Twitch are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the Top 5 Games from Twitch:

```js
const response = await window.casthub.fetch({
    integration: 'twitch',
    method: 'GET',
    url: 'kraken/games/top',
    data: {
        limit: 5,
    },
});
```

This supports both the Kraken and Helix APIs:

- <a href="https://dev.twitch.tv/v5" target="_blank" rel="noopener noreferer">Kraken</a>
- <a href="https://dev.twitch.tv/api/reference" target="_blank" rel="noopener noreferer">Helix</a>

### Chat

All you need to open a new connection is an Identity ID, provided by the User during installation.

```js
async mounted() {
    await super.mounted();

    const { id } = this.identity;

    // Connect to the Chat System.
    const chat = await window.casthub.chat(id);

    // And listen for new messages.
    chat.on('message', data => {
        console.log('NEW MESSAGE', data);
    });
}
```

If a connection to that specific Identity is already open (For example, when another Module/Automation has already opened it), you will be subscribed to events from that instance straight away and won't need to wait for a new connection to open.

### PubSub

CastHub makes using Twitch PubSub easy by auto-subscribing you to any Topics you listen for. For example:

```js
async mounted() {
    await super.mounted();

    const { id, identity } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    /**
     * CastHub will detect you're listening to this Topic and will automatically
     * send the `LISTEN` packet to Twitch.
     */
    ws.on(`channel-points-channel-v1.${identity}`, data => {
        console.log('NEW EVENT', data);
    });
}
```

If a connection to that specific Identity is already open (For example, when another Module/Automation has already opened it), you will be subscribed to events from that instance straight away and won't need to wait for a new connection to open.

#### Events

Events are unchanged from the regular Twitch documented PubSub Events, apart from an additional event for `follower`, since there is no real "nice" way of fetching or listening to Twitch Follower events.

This event exposes raw user data for _new_ followers, or at least the last 100 new followers in the last 5 seconds. For example:

```js
async mounted() {
    await super.mounted();

    const { id } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    ws.on('follower', data => {
        console.log(data);
    });
}
```

You can find the other supported Events from the <a href="https://dev.twitch.tv/pubsub#topics" target="_blank" rel="noopener noreferer">Twitch documentation.</a>

## Twitter

Code requiring an integration for Twitter are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the current User from Twitter:

```js
const { identity } = this.identity;

const response = await window.casthub.fetch({
    integration: 'twitter',
    method: 'GET',
    url: 'users/show',
    data: {
        user_id: identity,
    },
});
```

This supports the <a href="https://developer.twitter.com/en/api-reference-index" target="_blank" rel="noopener noreferer">Twitter API</a>.

## XSplit Broadcaster

CastHub offers an managed communication method between the Users' computer and XSplit Broadcaster. For example:

```js
async mounted() {
    await super.mounted();

    const { id } = this.identity;

    // Connect to the external WebSocket.
    const ws = await window.casthub.ws(id);

    // Set the initial active Scene.
    const { id } = await ws.send('getActiveScene');
    this.setActive(id);

    // Set the active Scene whenever it changes.
    ws.on('scenechange', ({ id }) => {
        this.setActive(id);
    });
}
```

If a connection to that specific Identity is already open (For example, when another Module/Automation has already opened it), you will be subscribed to events from that instance straight away and won't need to wait for a new connection to open.

Since this implementation is currently strictly internal, documentation on the given events cannot be provided.

## YouTube

Code requiring an integration for YouTube are given access to make requests on the Users' behalf. **All requests are automatically authenticated.**

Here is an example of requesting the statistics for the currently authenticated User from YouTube:

```js
const response = await window.casthub.fetch({
    integration: 'youtube',
    method: 'GET',
    url: 'channels',
    data: {
        part: 'statistics',
        mine: 'true',
    },
});
```

This supports the <a href="https://developers.google.com/youtube/v3" target="_blank" rel="noopener noreferer">YouTube Data API v3 reference</a>.

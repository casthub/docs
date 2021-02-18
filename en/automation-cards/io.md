---
title: I/O
description: When a Trigger is executed, it can optionally define an object to pass to future Conditions and Actions
position: 19
category: Automation Cards
---

When a [Trigger](/automation-cards/triggers) is executed, it can optionally define an object to pass to future [Conditions](/automation-cards/conditions) and [Actions](/automation-cards/actions). This is the I/O Object, and it has a loosely defined schema that is enforced by the App, but otherwise is open to any input.

The given schema by default, subject to change, is as follows:

```json
{
    "subject": null,
    "integration": null,
    "trigger": {
        "name": ""
    },
    "variables": {},
    "meta": {}
}
```

The following sections will break-down the schema, and is recommended for Developers to follow in order to create an ecosystem that works relatively easily with other Cards.

When marked as <badge>Automatic</badge>, you cannot override the property as it is set by the App itself.

## `subject`

Useful when a Trigger is set-off by a definable entity, such as a User. For example, if your Trigger listened to Twitch Subscriptions, the `subject` would be the ID/Username/User Object for the subscriber. Or if the Trigger is executed by an OBS Scene switch, the `subject` would be the ID/Name of that Scene.

## `integration`

<badge>Automatic</badge>

If the Trigger is added using a service, e.g. Twitch, this will be the slug for that service (In this example, `twitch`).

## `trigger`

<badge>Automatic</badge>

This Object provides information on the Trigger itself, currently only the unique `name` of the Trigger. This is the `key` value for that Trigger, for example, `obs-scene-changed`.

## `variables`

These should be simple key => string values. This is useful for Actions that want to use Trigger output in templates. For example:

```json
{
    "variables": {
        "username": "CastHub",
        "amount": "$10.00",
        "message": "Hello world!"
    }
}
```

## `meta`

Completely up to you! This field exists purely for you to pass down to other Cards that likely filter out non-compatible Triggers. Useful if you're developing a bespoke set of Cards, but still want them to work and execute when other non-compatible Cards are present in an Automation.

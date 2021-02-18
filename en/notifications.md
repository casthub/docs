---
title: Notifications
description: To provide feedback to users, you can use our Notifications API to send text-based alerts to the user
position: 22
category: Resources
---

To provide feedback to users, you can use our Notifications API to send text-based alerts to the user.

If the CastHub Window is minimized or not focussed, an Operating System Notification will be shown to the user with the text. An in-app Snackbar will be shown regardless of whether the window is focussed/visible.

```js
window.casthub.notify('Hello world!');
```

We plan on adding richer options, such as images, to Notification in the near-future.

---
title: "Create an Amazon Pinpoint Service"
chapter: false
weight: 19
---

Now let's create an Amazon Pinpoint service using the amplify command.

```bash
amplify add notifications
```

Select **FCM** as the notification channel as shown below.

ApkKey copies and pastes the **server key** of Cloud Messaging you created on the previous page.

![Example Service](/images/addnoti.png)

Push the created service.

```bash
amplify push
```


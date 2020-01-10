---
title: "Send Push message"
chapter: false
weight: 30
---

Push messages can be sent via pinpoint's campaign.
In a shell, run:  (You can also go directly from the console.)

```bash
amplify notifications console
```

Navigate to <b> AWS console> service> Pinpoint> campaigns> create a campaign </b>.

Enter **first-push** in Campaign name and go to **Next**.



![Example Service](/images/cam1.png)

Select **Create a segment** and name it **myGroup**. Go to the **Next** step. On the right, **3 endpoints** is the number of mobile devices on which your app is currently active. The number of mobile devices targeted by the segment group. Modifying the filter changes the number of endpoints.

![Example Service](/images/cam2.png)

Fill in **title** and **Body** of **Push notification details** and go to **Next** step.

![Example Service](/images/cam3.png)

Leave the default and go to the **Next** step.

![Example Service](/images/cam4.png)

Press **Launch campaign** at the bottom to send the created push message to each device.

![Example Service](/images/cam5.png)

The following figure shows a push message received.

<img src="/images/cam6.png" width="30%" hight="30%">

---
title: "Setting up Firebase Cloud Messaging (FCM)"
date: 2018-08-07T08:30:11-07:00
weight: 10
---



Amazon Pinpoint uses Firebase Cloud Messaging (FCM) to send push notifications. To do this, you need to create a firebase project.

Go to https://console.firebase.google.com/

If you don't have a Google account, create one and sign in if it's signed out.



![Example Service](/images/firebase-welcome.png)



Select **Create a project**. Enter a project name and press **Continue** to move on to the next step.

![Example Service](/images/firebase-create-1.png)

Since this workshop will only use FCM for push message, disable **Emable Google Analytics for this project** and move on to the next step.

![Example Service](/images/firebase-create-2.png)

When the project is created, the following screen will be displayed. Select the gear icon in the top left corner with the **Settings** icon and select **Project settings**.


![Example Service](/images/firebase-create-3.png)

The **General** tab has **Your apps** at the bottom. Select the icon of android.

![Example Service](/images/firebase-addproject-1.png)

Enter the Package name (**com.example.socialandroidapp**) and nickname (**aws-android-workshop**) as shown below. Press the **Register app** button to move on to the next step.

![Example Service](/images/firebase-addproject-2.png)

Press the **Download** button on the screen to download the json file. Refer to the picture for the path to download.

![Example Service](/images/firebase-addproject-3.png)

After that, select all as default and complete.


Back in Settings, select the second tab, **Cloud Messaging**.

Copy the value corresponding to **Server key**. You will need it in the next chapter.

![Example Service](/images/firebase-addproject-4.png)

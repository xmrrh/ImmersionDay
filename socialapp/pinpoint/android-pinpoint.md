---
title: "Writing Android Code for Push Notifications  "
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 20
---

Now let's work with the Android project to receive Push Notifications.

First add dependencies to build.gradle (Module: Project) as shown below.

```java
dependencies {
        ...
        classpath 'com.google.gms:google-services:4.0.1'
        ...
    }
```



Apply the plugin to build.gradle (Module: app)

```java
apply plugin: 'com.google.gms.google-services'
```



Also add the following dependency to -build.gradle (Module: app)-in the same file.

```java
dependencies {
...
// Overrides an auth dependency to ensure correct behavior
     implementation 'com.google.android.gms:play-services-auth:15.0.1'

     implementation 'com.google.firebase:firebase-core:16.0.1'
     implementation 'com.google.firebase:firebase-messaging:17.3.0'

     implementation 'com.amazonaws:aws-android-sdk-pinpoint:2.15.+'
...
}
```



Go to AndroidManifest.xml and define Push Listener Service.

```xml
<application>
        ...
        <service
            android:name=".PushListenerService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT"/>
            </intent-filter>
        </service>
</application>
```

Press Push notification to open the app. To do this, add Receiver to AndroidManifest.xml. 

```xml
<application>
        ...
        <receiver android:name="com.amazonaws.mobileconnectors.pinpoint.targeting.notification.PinpointNotificationReceiver" android:exported="false" >
        <intent-filter>
            <action android:name="com.amazonaws.intent.fcm.NOTIFICATION_OPEN" />
        </intent-filter>
    </receiver>
</application>
```





Go to MainActivity and write code to create an Amazon Pinpoint client.

```java
public class MainActivity extends AppCompatActivity {
  ...
private static PinpointManager pinpointManager;

    public static PinpointManager getPinpointManager(final Context applicationContext) {
        if (pinpointManager == null) {
            final AWSConfiguration awsConfig = new AWSConfiguration(applicationContext);
            AWSMobileClient.getInstance().initialize(applicationContext, awsConfig, new Callback<UserStateDetails>() {
                @Override
                public void onResult(UserStateDetails userStateDetails) {
                    Log.i("INIT", userStateDetails.getUserState().toString());
                }

                @Override
                public void onError(Exception e) {
                    Log.e("INIT", "Initialization error.", e);
                }
            });

            PinpointConfiguration pinpointConfig = new PinpointConfiguration(
                    applicationContext,
                    AWSMobileClient.getInstance(),
                    awsConfig);

            pinpointManager = new PinpointManager(pinpointConfig);

            FirebaseInstanceId.getInstance().getInstanceId()
                    .addOnCompleteListener(new OnCompleteListener<InstanceIdResult>() {
                        @Override
                        public void onComplete(@NonNull Task<InstanceIdResult> task) {
                            if (!task.isSuccessful()) {
                                Log.w(TAG, "getInstanceId failed", task.getException());
                                return;
                            }
                            final String token = task.getResult().getToken();
                            Log.d(TAG, "Registering push notifications token: " + token);
                            pinpointManager.getNotificationClient().registerDeviceToken(token);
                        }
                    });
        }
        return pinpointManager;
    }
  ...
}
```
Import the required class. If you need more classes, import the classes using the Option & Enter or Alt & Enter key combinations as before.

```java
 import com.amazonaws.mobileconnectors.pinpoint.PinpointConfiguration;
 import com.amazonaws.mobileconnectors.pinpoint.PinpointManager;
 import android.content.Context;
 import com.amazonaws.mobile.config.AWSConfiguration;
 import com.amazonaws.mobile.client.UserStateDetails;
 import com.amazonaws.mobile.client.Callback;
 import com.amazonaws.mobile.client.AWSMobileClient;
 import com.google.android.gms.tasks.OnCompleteListener;
 import com.google.android.gms.tasks.Task;
 import com.google.firebase.iid.FirebaseInstanceId;
 import com.google.firebase.iid.InstanceIdResult;

```


Initialize it using the getPinpointManager function you just created in the onCreate function of MainActivity.

MainActivity.java 

```java
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ...
        // Initialize PinpointManager
        getPinpointManager(getApplicationContext());
    	  ...
    }

```



Create a PushListenerService.java.  This service receives and processes Push messages.

```java
package com.example.socialandroidapp;
import android.content.Intent;
 import android.os.Bundle;
 import androidx.localbroadcastmanager.content.LocalBroadcastManager;
 import android.util.Log;

 import com.amazonaws.mobileconnectors.pinpoint.targeting.notification.NotificationClient;
 import com.amazonaws.mobileconnectors.pinpoint.targeting.notification.NotificationDetails;
 import com.google.firebase.messaging.FirebaseMessagingService;
 import com.google.firebase.messaging.RemoteMessage;

 import java.util.HashMap;

 public class PushListenerService extends FirebaseMessagingService {
     public static final String TAG = PushListenerService.class.getSimpleName();

     // Intent action used in local broadcast
     public static final String ACTION_PUSH_NOTIFICATION = "push-notification";
     // Intent keys
     public static final String INTENT_SNS_NOTIFICATION_FROM = "from";
     public static final String INTENT_SNS_NOTIFICATION_DATA = "data";

     @Override
     public void onNewToken(String token) {
         super.onNewToken(token);

         Log.d(TAG, "Registering push notifications token: " + token);
         MainActivity.getPinpointManager(getApplicationContext()).getNotificationClient().registerDeviceToken(token);
     }

     @Override
     public void onMessageReceived(RemoteMessage remoteMessage) {
         super.onMessageReceived(remoteMessage);
         Log.d(TAG, "Message: " + remoteMessage.getData());

         final NotificationClient notificationClient = MainActivity.getPinpointManager(getApplicationContext()).getNotificationClient();

         final NotificationDetails notificationDetails = NotificationDetails.builder()
                 .from(remoteMessage.getFrom())
                 .mapData(remoteMessage.getData())
                 .intentAction(NotificationClient.FCM_INTENT_ACTION)
                 .build();

         NotificationClient.CampaignPushResult pushResult = notificationClient.handleCampaignPush(notificationDetails);

         if (!NotificationClient.CampaignPushResult.NOT_HANDLED.equals(pushResult)) {
             /**
                The push message was due to a Pinpoint campaign.
                If the app was in the background, a local notification was added
                in the notification center. If the app was in the foreground, an
                event was recorded indicating the app was in the foreground,
                for the demo, we will broadcast the notification to let the main
                activity display it in a dialog.
             */
             if (NotificationClient.CampaignPushResult.APP_IN_FOREGROUND.equals(pushResult)) {
                 /* Create a message that will display the raw data of the campaign push in a dialog. */
                 final HashMap<String, String> dataMap = new HashMap<>(remoteMessage.getData());
                 broadcast(remoteMessage.getFrom(), dataMap);
             }
             return;
         }
     }

     private void broadcast(final String from, final HashMap<String, String> dataMap) {
         Intent intent = new Intent(ACTION_PUSH_NOTIFICATION);
         intent.putExtra(INTENT_SNS_NOTIFICATION_FROM, from);
         intent.putExtra(INTENT_SNS_NOTIFICATION_DATA, dataMap);
         LocalBroadcastManager.getInstance(this).sendBroadcast(intent);
     }

     /**
      * Helper method to extract push message from bundle.
      *
      * @param data bundle
      * @return message string from push notification
      */
     public static String getMessage(Bundle data) {
         return ((HashMap) data.get("data")).toString();
     }
 }
```

The code is now complete. Press **Run button** at the top of your Android Studio project to run it with the emulator you've already created. After execution, move to MainActivity screen.

![c9after](/images/run.png)


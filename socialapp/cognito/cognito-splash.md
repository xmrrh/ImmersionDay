---
title: "Creating a splash screen"
date: 2018-08-07T08:30:11-07:00
weight: 11
---

In this tutorial, we will configure a splash page of the app. On the splash page, code is added to check the login status. If there is no existing login information, the page will be moved to the **LOGIN MAIN** page, and if there is login information, the page will be moved to the **APP MAIN** page.

Copy the code snippet below to complete the **_ initCognito** method.

```java
// SplashActivity.java

public class SplashActivity extends AppCompatActivity {
    ...

    private void _initCognito() {
        // Add code here
        if (AWSMobileClient.getInstance().getConfiguration() == null){
            // Initialize user
            AWSMobileClient.getInstance().initialize(getApplicationContext(), new Callback<UserStateDetails>() {
                @Override
                public void onResult(UserStateDetails userStateDetails) {
                    switch (userStateDetails.getUserState()){
                        case SIGNED_IN:
                            // Open Main Activity
                            CommonAction.openMain(context);
                            break;
                        case SIGNED_OUT:
                            Log.d(TAG, "Do nothing yet");
                            CommonAction.openAuthMain(context);
                            break;
                        default:
                            AWSMobileClient.getInstance().signOut();
                            break;
                    }
                }

                @Override
                public void onError(Exception e) {
                    Log.e("INIT", e.toString());
                }
            });

        } else if (AWSMobileClient.getInstance().isSignedIn()){
            // Logined user
            CommonAction.openMain(context);
        } else {
            // Logouted user
            CommonAction.openAuthMain(context);
        }

    }

    ...
}
```
Press **Run** button at the top of your Android Studio IDE to run the application with the emulator you have already created.
![c9after](/images/run.png)

If the app runs properly, you will see a splash screen. And login status checking logic will be run in the background.
![Example Service](/images/amplify-auth-splash.png)

If there is no login state, the page automatically will be moved to the **LOGIN MAIN** page.
![Example Service](/images/amplify-auth-login-main.png)

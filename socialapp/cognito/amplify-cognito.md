---
title: "Configuring Amazon Cognito Service"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Amazon Cognito allows you to quickly and easily add user sign-up, sign-in, and access control to your mobile app.

Run the following command from the top level of your Android project.

```shell
amplify add auth
```

Please refer to the picture below for the selection in progress.
![Example Service](/images/amplify-auth-cli-setting.png)

When finished, build all your local backend resources and provision it in the cloud with the following command:
```shell
amplify push
```

Press 'Y' to continue. It will take minutes to complete.
![Example Service](/images/amplify-cognito-push.png)

To verify that the Cognito User Pool has been created, access the AWS Management Console(AWS console> Services> Cognito> User Pools).
![Example Service](/images/amplify-cognito-console.png)

You can also see that each configuration value is created in res/raw/awsconfiguration.json file in Android Studio.
![Example Service](/images/amplify-auth-android-config.png)





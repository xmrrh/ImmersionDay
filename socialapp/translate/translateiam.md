---
title: "IAM Role"
chapter: false
weight: 15
---

The reason for not being translated is the following error in logcat: **AccessDenied**.

```verilog
2019-09-04 18:00:40.159 10974-11061/com.example.socialandroidapp E/dev-day-item: Error occurred in translating the text: User: arn:aws:sts::539063931014:assumed-role/aws-android-workshop-dev-20190903125208-authRole/CognitoIdentityCredentials is not authorized to perform: translate:TranslateText (Service: AmazonTranslate; Status Code: 400; Error Code: AccessDeniedException; Request ID: af77f01a-f368-4987-96f0-780c7c6ca26d)



```

There was an IAM role created with the Amplify init. This role can be solved by giving Translate permission.

<b> AWS console> service> IAM> Roles> aws-android-workshop-dev </b> in the search box and select **auth role**.

![Example Service](/images/iamauthrole_eng.png)

Press **Attach Policies** on the Permissions tab

![Example Service](/images/permission1_eng.png)

Enter **Translate** in the filter policies. Select **TranslateFullAccess** from the search results and click **Attach Policy**.


<img src="/images/search-translate.png">

The final figure is as follows.

<img src="/images/iam-translate.png" >

Now go back to the application and click the **TRANSLATE** button. You can see the change to the specified language. 

<img src="/images/result.png" width="30%" hight="30%">


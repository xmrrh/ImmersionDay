---
title: "IAM Role"
chapter: false
weight: 13
---

Posts made on the Write screen are not added to the table created in <b> AWS console> service> Dynamodb </b>. The Error log like below. It means upload fail due to **AccessDenied** for S3.

```verilog
2019-09-04 11:13:55.305 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:55.321 8048-8057/com.example.socialandroidapp W/System: A resource failed to call close. 
2019-09-04 11:13:55.330 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:55.334 8048-8081/com.example.socialandroidapp I/chatty: uid=10085(com.example.socialandroidapp) RenderThread identical 1 line
2019-09-04 11:13:55.366 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:57.057 8048-8153/com.example.socialandroidapp D/com.amazonaws.request: Received error response: com.amazonaws.services.s3.model.AmazonS3Exception: Access Denied (Service: null; Status Code: 403; Error Code: AccessDenied; Request ID: 80D7D91725581D4C), S3 Extended Request ID: dY+iw43vR8nE8DDB4L3F4/ijgC/Ydts/KKgwpwTcplUrtWTuN9GBfAxWcQNIgLRgqW0uR1OEYmA=
2019-09-04 11:13:57.059 8048-8153/com.example.socialandroidapp V/InterceptorCallback: Thread:[1086]: onFailure() S3 upload failed.

```

There was an IAM role created with the Amplify init. This role can be solved by giving S3 permission.

<b> AWS console> service> IAM> Role> aws-android-workshop-dev </b> in the search box and select **auth role**.

![Example Service](/images/iamauthrole_eng.png)

Press **Attach Policies** on the Permissions tab

![Example Service](/images/permission1_eng.png)

Enter **S3** in the filter policies. Select **AmazonS3FullAccess** from the search results and click **Attach Policy**.

![Example Service](/images/permission2_eng.png)

The final figure is as follows.

![Example Service](/images/permission3_eng.png)

Now go back to the application **logout and then log in again**, write a post.

After posting, you can see the new item has been added to DynamoDB.

![Example Service](/images/dynamodb_eng.png)

You can see the photo uploaded on S3 as well.

![Example Service](/images/s3-upload_eng.png)

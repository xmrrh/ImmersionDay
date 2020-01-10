---
title: "Amplify CLI Install"
chapter: false
weight: 1
---

AWS Amplify makes it easy to create, configure, and implement scalable mobile and web apps based on AWS.  AWS Amplify provisions and manages backends for mobile applications.

Simply select the features you need, such as authentication, analytics, and offline data synchronization, and Amplify automatically provisions and manages the AWS services that provide each. 

Use npm to install the Amplify CLI. If npm is not installed

Install via https://nodejs.org/en/download/




Install the Amplify CLI.

```bash
npm install -g @aws-amplify/cli
```

If it is not installed, proceed as follows : 

```bash
sudo npm install -g --unsafe-perm --verbose @aws-amplify/cli
```



Configure Amplify.

```bash
amplify configure 
```

This command will ask you to login to AWS console account with your login credentials by opening a new browser window. Once logged in, go back to terminal window and press **enter** to continue, again.

This workshop region is us-east-1. Enter **us-east-1** as the Default Region.

In User name, enter the IAM user you want to create. 


![Create User](/images/amplify-configure-1.png)

On entering the username, press **enter** and it will open AWS console again in a browser window for you to add a user.

![Example Service](/images/adduser-1.png)

Press **next** to move to the screen shown below.(Last screen)

Save **Download .csv** to your local PC.

![Example Service](/images/adduser-2.png)





Press **Enter** and type the Access Key followed by the Secret Access Key. Access Key and Secret Access Key can be imported from the downloaded csv file or add user screen.



You can use Profile Name as default or give it a name and use it as the –profile option. In this case, we put **us-east-1-profile**. (You can enter any name you like.)



![Create User](/images/awsconfigure.png)


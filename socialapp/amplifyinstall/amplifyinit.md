---
title: "Amplify Initialization"
chapter: false
weight: 2
---

Move to the root directory of the downloaded sample app and initialize the Amplify project.

```shell
cd aws-android-workshop/
amplify init
```
Press **Enter** to accept the default project name

For the name of the environment you can either choose the name of the operating system you are working on or the environments such as dev, local or production. Enter **dev** for the environment name.

Select a convenient one for the default editor. ( For example,  **vim** ).

Choose **Android** when prompted

**Accept** the default values for res directory.

Choose to use AWS Profile (<b> Y </b>) and select the profile you have already created. **us-east-1-profile**

Please refer to the picture below and enter. 

![Example Service](/images/amplifyinit.png)



When the Amplify init is done, IAM roles are automatically created for authenticated and unauthenticated users. You can see two IAM roles created as below.

<b>AWS console > service > IAM > Roles > </b> <br>
Type "Aws-android-workshop-dev" in the search box

![Example Service](/images/iamauthrole_eng.png)



You can also see that res/raw/awsconfiguration.json file is created in Android Studio.

![Example Service](/images/jsonfile.png)



You can now create various AWS services using the Amplify CLI command.

command can be used with:

- `amplify add <category>`
- `amplify remove <category>`
- `amplify push <category>`


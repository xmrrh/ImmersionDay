---
title: "Fuzzy App Test"
chapter: false
weight: 22
---



In this step, you will run a simple test on multiple devices using the Fuzzy test.

⦁	To create a test project, return to the AWS Console and select Device Farm from the list of services.
⦁	Select '+Create a New project' on the screen.
⦁	In the Create project popup, enter the appropriate Project name.
⦁	You will enter the test project screen with the Success statement. And created a Device Farm project for testing your app.

⦁	Choose Create a new run.

![Create User](/images/fuzzy1.png)

⦁	Click on the Android and Apple icons, select Upload from the Spending section below and select the apk file you created earlier. You can also drag and drop files into the upload area.

![Create User](/images/fuzzy2.png)

⦁	Once the upload is complete, basic information about the app will be displayed. Information such as the domain name of the package, the main activity name, the minimum supported SDK version, and the supported screen size are listed. Choose Next step.

![Create User](/images/fuzzy3.png)


⦁	Configure item appears and it is set as Built-in: Fuzz by default in Test item. Event Count specifies a number between 1 and 10,000 that indicates how many user interface events the test will perform. Event throttle specifies a number between 1 and 1,000 that represents the number of milliseconds the purge test waits before performing the next user interface event. Randomizer seed specifies the number to use when the fuzzy test randomizes user interface events. Specifying the same number for subsequent fuzzy tests ensures the same sequence of events.

![Create User](/images/fuzzy4.png)

⦁	In the Fuzz test, set the default value and proceed. Click Next step.
⦁	Select devices. By default it uses a pre-configured list of the most popular models as Top Devices. You will see a preview of whether the uploaded app is compatible with your device. Depending on the case, you can configure test list presets according to the desired OS version and form factor in Create a new device pool. Go to the next step.

![Create User](/images/fuzzy5.png)
![Create User](/images/fuzzy6.png)

⦁	The 'Specify device state' page allows you to configure additional files and additional apps needed to run or test your app. You can also turn options such as WIFI and Bluetooth on or off. You can optionally specify the Latitude and Longitude as the Device location option. Leave all other values as Default and move on to the next step.
⦁	In 'Review and start run', you can set the maximum time to run. This prevents the devices to be tested from running for long periods of time with exceptions or unexpected behavior. In this demo, there are not many features of the app, and since the test is completed in a short time, we will set it to 5 minutes, which is the minimum operation time. Five minutes of operation on five devices takes 25 minutes. Start the Fuzz test with Confirm and start run.

![Create User](/images/fuzzy7.png)


⦁	Fuzz Test Measurement and Results

⦁	After completing the above steps, you will be returned to the project test page, indicating that the app is running a test. It will take the maximum time set before to complete the operation.

![Create User](/images/fuzzy8.png)


⦁	Pressing the 'Run List' to enter the details will show the execution results for each device and the screen shots generated in the test step. Looking at Summery, you can easily see the data points for each device.
⦁	If you click on one of the device list to go to details, you can view the video of the testing process, and you can view or download the details and logs of each test step.
⦁	In particular, you can check the performance issues that may occur by showing the CPU, Memory usage and Threads indicators in detail.

![Create User](/images/fuzzy9.png)

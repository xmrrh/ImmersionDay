---
title: "Implementing e-mail based login"
date: 2018-08-07T08:30:11-07:00
weight: 14
---

In this exercise, we will implement an email-based login.

Copy the code below to complete the **_signIn** method, which performs an email-based login.
```java
// LoginActivity.java
public class LoginActivity extends AppCompatActivity implements Validator.ValidationListener {

    ...

    private void _signIn(String userName, String password) {
        AWSMobileClient.getInstance().signIn(userName, password, null, new Callback<SignInResult>() {
            @Override
            public void onResult(final SignInResult signInResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-in callback state: " + signInResult.getSignInState());
                    switch (signInResult.getSignInState()) {
                        case DONE:
                            makeToast(context,"Sign-in done.");
                            CommonAction.openMain(context);
                            break;
                        case SMS_MFA:
                            makeToast(context, "Please confirm sign-in with SMS.");
                            break;
                        case NEW_PASSWORD_REQUIRED:
                            makeToast(context, "Please confirm sign-in with new password.");
                            break;
                        default:
                            makeToast(context, "Unsupported sign-in confirmation: " + signInResult.getSignInState());
                            break;
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Sign-in error", e);
                runOnUiThread(() -> {
                    if (e instanceof AmazonServiceException)
                        makeToast(context, ((AmazonServiceException) e).getErrorMessage());
                });
            }
        });
    }

    ...

}

```

Now let's run the code.

Press **Run** button at the top of your Android Studio IDE to run the application with the emulator you have already created.
![c9after](/images/run.png)

When the following screen appears, click the **Email login** text to go to the email-based login menu.
![Example Service](/images/amplify-auth-login-main.png)

Log in with the user information used in the previous tutorial.
![Example Service](/images/app-login.png)

If registration is successful, the page will be moved to the main screen of the app.
![Example Service](/images/app-main-empty.png)

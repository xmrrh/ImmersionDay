---
title: "Implementing e-mail based signup"
date: 2018-08-07T08:30:11-07:00
weight: 13
---

In this tutorial, we will implement email-based signup with client APIs provided by AWSMobileClient.

Before starting the lab, we need to access the AWS Conginto management console to verify that email is selected as the login method. Verify that the contents of the Attributes item in the AWS Management Console are set up as shown below.
![Example Service](/images/amplify-auth-congito-attr.png)

Now let's step through the code for email-based signup.

1. Sign up using the email and password. <br>Copy the code below to complete the **signUp** method for signing up. The signUp method adds a new user to the Cognito user pool with the email and password entered. If the user information is registered properly, the screen switches to SignUpConfirmFragment for user email authentication.
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {
    
    ...

    @Override
    public void signUp(String email, String password) {
        userName = email;
        this.password = password;

        // Add code here
        final Map<String, String> attributes = new HashMap<>();
        attributes.put("email", email);
        AWSMobileClient.getInstance().signUp(userName, password, attributes, null, new Callback<SignUpResult>() {
            @Override
            public void onResult(final SignUpResult signUpResult) {
                runOnUiThread(() -> {
                    if (!signUpResult.getConfirmationState()) {
                        final UserCodeDeliveryDetails details = signUpResult.getUserCodeDeliveryDetails();
                        makeToast(context, "Confirm sign-up with: " + details.getDestination());
                        setSignUpConfirmFragment();
                    } else {
                        makeToast(context, "Sign-up done.");
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Sign-up error", e);
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

2. Email verification. <br>If you successfully signed up in Step 1, you will receive a verification code by email. Copy the code below to complete the **confirmSignUp** method, which completes the signup phase by entering the verification code received from the email.
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {

    ...

    @Override
    public void confirmSignUp(String code) {
        // Add code here
        AWSMobileClient.getInstance().confirmSignUp(userName, code, new Callback<SignUpResult>() {
            @Override
            public void onResult(final SignUpResult signUpResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-up callback state: " + signUpResult.getConfirmationState());
                    if (!signUpResult.getConfirmationState()) {
                        final UserCodeDeliveryDetails details = signUpResult.getUserCodeDeliveryDetails();
                        makeToast(context,"Confirm sign-up with: " + details.getDestination());
                    } else {
                        makeToast(context, "Sign-up done.");
                        // SignIn and move to MainActivity
                        _signIn(userName, password);
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Confirm sign-up error", e);
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

3. Auto-Login. <br>Once email verification is complete, complete the **_ signIn** function, which performs an automatic login using the email and password we used during the sign up phase.
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {

    ...

    private void _signIn(String username, String password) {
        // Add code here
        AWSMobileClient.getInstance().signIn(username, password, null, new Callback<SignInResult>() {
            @Override
            public void onResult(final SignInResult signInResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-in callback state: " + signInResult.getSignInState());
                    switch (signInResult.getSignInState()) {
                        case DONE:
                            makeToast(context, "Sign-in done.");
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

When the following screen appears, click the **Sign up** text to move to the email-based sign up menu.
![Example Service](/images/amplify-auth-login-main.png)

When the following screen appears, enter your email and password to register. Please use a valid email account because an email verification process is required.
![Example Service](/images/app-signup.png)

Check your email inbox to see the verification code sent by the Cognito service.
![Example Service](/images/auth-email-verfication.png)

In the verification code input screen, enter the verification code delivered by email.
![Example Service](/images/app-email-verification.png)

If registration is successful, the page will be moved to the main screen of the app. On the first run, if a permission check dialog box appears, click ** ALLOW **.
![Example Service](/images/app-main-init.png)

Registered user information can be found through the Cognito menu in the AWS Management Console.
![Example Service](/images/auth-cognito-email-user.png)


After completing the lab, log out for the next tutorial.

1. Select the **SETTINGS** button on the app main screen
![Example Service](/images/app-main-empty.png)
2. Select the **LOGOUT** button on the app settings screen
![Example Service](/images/app-settings.png)

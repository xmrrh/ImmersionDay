---
title: "Integrating Translate Service"
chapter: false
weight: 13

---

Add dependency to build.gradle (Module: app) as shown below.

```xml
dependencies {
...
    implementation 'com.amazonaws:aws-android-sdk-translate:2.13.+'

}
```

The TRANSLATE button is located on each post. Registers a button click event and listener in bindData () of PostAdapter for translation when the TRANSLATE button is pressed.

```java
void bindData(final ListPostsQuery.Item item) {

            titleTxt.setText(item.title());
  ...
  //add 
            translateBtn.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v) {
                    doTranslate(contentsTxt);
                    doTranslate(titleTxt);
                }
            });
        }

```

Write the function responsible for the actual translation under the **bindData () function**. AmazonTranslateAsyncClient authenticates using AWSCredential from Cognito.

```java
 private void doTranslate(final TextView tv) {
        AmazonTranslateAsyncClient translateAsyncClient = new AmazonTranslateAsyncClient(ClientFactory.getAWSCredentials());
        TranslateTextRequest translateTextRequest = new TranslateTextRequest()
                .withText(tv.getText().toString()).withTargetLanguageCode(Util.getLanguageCode(ctx))
                .withSourceLanguageCode("auto");

        translateAsyncClient.translateTextAsync(translateTextRequest, new AsyncHandler<TranslateTextRequest, TranslateTextResult>() {
            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Error occurred in translating the text: " + e.getLocalizedMessage());
            }

            @Override
            public void onSuccess(TranslateTextRequest request, TranslateTextResult translateTextResult) {
                tv.setText(translateTextResult.getTranslatedText());

            }
        });

    }
```
Import the required class.

```java
import com.amazonaws.services.translate.AmazonTranslateAsyncClient;
import com.amazonaws.services.translate.model.TranslateTextRequest;
import com.amazonaws.services.translate.model.TranslateTextResult;
import com.amazonaws.handlers.AsyncHandler;
```

Press **Run** at the top of your Android Studio project to run the application with the emulator you have already created.

![c9after](/images/run.png)

Access the screen via the **SETTINGS** button on the main screen.

<img src="/images/main-list.png" width="30%" hight="30%">

Select the language you want to translate from the list box in Preferred language setting. (Ex, korean)

<img src="/images/language.png" width="30%" hight="30%">

**save** and return to the main screen. Press the **Translate** button to see if the translation is available. It is correct that no translation.

<img src="/images/main-list.png" width="30%" hight="30%">






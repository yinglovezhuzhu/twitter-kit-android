TweetComposer provides two ways to compose Tweets:

#. Launch the Twitter application's Tweet Composer - a feature-rich composer which supports attaching images and videos.
#. Launch the Twitter Kit Native Composer - a lightweight composer which lets users compose Tweets from within your application.

# Launching Twitter Composer

The Twitter Android application allows apps to start the Tweet composer via an Intent. The Twitter Android composer is feature-rich, familiar to users, and has options for attaching images and videos.

## Build an Intent

Start construction of a Tweet composer by using the TweetComposer Builder.

``` java

   import com.twitter.sdk.android.tweetcomposer.TweetComposer;
   ...

   TweetComposer.Builder builder = new TweetComposer.Builder(this)
        .text("just setting up my Twitter Kit.")
        .image(imageUri);
   builder.show();
```
The image `Uri` should be a `Uri` using the `content://` scheme. For example,

```java

    Uri imageUri = FileProvider.getUriForFile(MainActivity.this,
      BuildConfig.APPLICATION_ID + ".file_provider",
      new File("/path/to/image"));
```
![](http://twitter.github.io/twitter-kit-android/images/twitter-for-android-composer.png)


If the Twitter app is not installed, the intent will launch [twitter.com](https://twitter.com) in a browser, but the specified image will be ignored. For more details on correctly setting up a `FileProvider` see [Setup Sharing](https://developer.android.com/training/secure-file-sharing/setup-sharing.html).


# Twitter Kit Native Composer

The Twitter Kit Native Composer is a lightweight composer which lets users compose Tweets from within your application. It does not depend on the Twitter for Android app being installed.

![](http://twitter.github.io/twitter-kit-android/images/native-composer.png)

## Intent Builder

After authenticating a user, build an :code:`Intent` to start the TweetComposer's :code:`ComposerActivity`.

```java

    final TwitterSession session = TwitterCore.getInstance().getSessionManager()
        .getActiveSession();
    final Intent intent = new ComposerActivity.Builder(YourActivity.this)
        .session(session)
        .image(uri)
        .text("Love where you work")
        .hashtags("#twitter")
        .createIntent();
    startActivity(intent);
```


| Builder Method          | Description                                    |
| ---------------------------- | ------------------------------------------- |
| session(TwitterSession) | Set the TwitterSession of the User to Tweet    |
| image(Uri)              | Attach an image to the Tweet                   |
| text(String)            | Text to prefill in composer                    |
| hashtags(String...)     | Hashtags to prefill in composer                |
| darkTheme()             | Use the dark composer theme, defaults to light |

# Results

After attempting to post a Tweet, the`TweetUploadService` broadcasts an Intent with the action value `com.twitter.sdk.android.tweetcomposer.UPLOAD_SUCCESS` for success, `com.twitter.sdk.android.tweetcomposer.UPLOAD_FAILURE` for failure or `com.twitter.sdk.android.tweetcomposer.TWEET_COMPOSE_CANCEL` for dismissing the compose dialog. On success, the `Intent` will contain an extra value with the Tweet ID of the created Tweet. On failure, the `Intent` will contain a copy of the original intent which could be used to retry the upload. For cancel there is no extra value.

You can create a `BroadcastReceiver` to receive these Intents.

```java

  public class MyResultReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
      if (TweetUploadService.UPLOAD_SUCCESS.equals(intent.getAction())) {
        // success
        final Long tweetId = intentExtras.getLong(TweetUploadService.EXTRA_TWEET_ID);
      } else if (TweetUploadService.UPLOAD_FAILURE.equals(intent.getAction())) {
        // failure
        final Intent retryIntent = intentExtras.getParcelable(TweetUploadService.EXTRA_RETRY_INTENT);
      } else if (TweetUploadService.TWEET_COMPOSE_CANCEL.equals(intent.getAction())) {
        // cancel
      }
    }
  }
```
Don’t forget to add your BroadcastReceiver to the application manifest.

```xml

  <receiver
    android:name=".MyResultReceiver"
    android:exported="false">
    <intent-filter>
      <action android:name="com.twitter.sdk.android.tweetcomposer.UPLOAD_SUCCESS"/>
      <action android:name="com.twitter.sdk.android.tweetcomposer.UPLOAD_FAILURE"/>
      <action android:name="com.twitter.sdk.android.tweetcomposer.TWEET_COMPOSE_CANCEL"/>
    </intent-filter>
  </receiver>
```
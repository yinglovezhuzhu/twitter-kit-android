The Android SDK is a collection of individual libraries designed to make interacting with Twitter seamless and efficient. This includes:

* TwitterCore for Twitter login and API access.
* TweetUi for displaying Tweets and Timelines.
* TweetComposer for creating Tweets.
* Twitter + MoPub for monetizing Twitter timelines with MoPub.

Twitter requires that all API requests be authenticated with tokens that can be traced back to an individual Twitter App. To create a new Twitter app or use existing Twitter app, visit `Twitter app dashboard <https://apps.twitter.com>`_ and copy the keys from the “Keys and Access Tokens” tab of your app page. You will need your consumer key and secret to complete the process.

# Add Twitter Kit Dependencies

To get started you will need to add the following dependencies to your application's Gradle config (usually `app/build.gradle`).

```groovy

    dependencies {
      // Include all the Twitter APIs
      compile 'com.twitter.sdk.android:twitter:3.1.1'
      // (Optional) Monetize using mopub
      compile 'com.twitter.sdk.android:twitter-mopub:3.1.1'
    }
```
In some cases it may be desirable to include only the necessary dependencies to avoid the Android dex limit.

```groovy

    dependencies {
      compile 'com.twitter.sdk.android:twitter-core:3.1.1'
      compile 'com.twitter.sdk.android:tweet-ui:3.1.1'
      compile 'com.twitter.sdk.android:tweet-composer:3.1.1'
      compile 'com.twitter.sdk.android:twitter-mopub:3.1.1'
    }
```
All the Twitter Kit artifacts are distributed through [Bintray jcenter](https://bintray.com/bintray/jcenter). Make sure jcenter is included in your repositories scope.

```groovy

    repositories {
      jcenter()
    }
```

# Initialize Twitter Kit


Once you’ve included the Twitter Kit dependencies you must initialize Twitter Kit. If using a custom :code:`Application` class you can initialize Twitter Kit in the :code:`onCreate()` method.

```java

    public class CustomApplication {
      public void onCreate() {
        Twitter.initialize(this);
      }
    }
```
Next, add your API key and secret to your application resources.

``` xml

    <resources>
      <string android:name="com.twitter.sdk.android.CONSUMER_KEY">XXXXXXXXXXX</string>
      <string android:name="com.twitter.sdk.android.CONSUMER_SECRET">XXXXXXXXXXX</string>
    </resources>
```

Optionally, you can change the default configuration by calling initialize with custom configuration.

```java

    public void onCreate() {
      TwitterConfig config = new TwitterConfig.Builder(this)
        .logger(new DefaultLogger(Log.DEBUG))
        .twitterAuthConfig(new TwitterAuthConfig("CONSUMER_KEY", "CONSUMER_SECRET"))
        .debug(true)
        .build();
      Twitter.initialize(config);
    }
```


| Builder Method| Description|
| ---------- | ---------------------- |
| logger(Logger)                       | Global logger to be used. Default log level is Log.INFO.                                       |

| twitterAuthConfig(TwitterAuthConfig) | Programmatically set consumer key and secret. By default value is read from Android Resources. |
| debug()                              | Enable debug mode.                                                                             |

Once Twitter kit has been initialized TwitterCore, TweetUi, and TweetComposer can be accessed through their singleton accessor.

- `TwitterCore.getInstance()`
- `TweetUi.getInstance()`
- `TweetComposer.getInstance()`

# Next Steps

Once you’ve installed Twitter, the following sections contain more detailed documentation for each feature.

* [Log In with Twitter](./Log-In-with-Twitter.html)
* [Compose Tweets](./Compose-Tweets.html)
* [Show Tweets](./Show-Tweets.html)
* [Show Timelines](./Show-Timelines.html)
* [Monetize with MoPub](./Monetize-With-MoPub.html)
* [Access Twitter’s REST API](Access-Twitter’s-REST-API.html)

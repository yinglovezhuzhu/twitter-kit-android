TwitterCore provides Log In with Twitter. This feature enables application users to authenticate with Twitter. Before using this feature, ensure that "Sign in with Twitter" is enabled for your Twitter app [Twitter Developer Console](https://apps.twitter.com).

When attempting to obtain an authentication token, TwitterCore will use the locally installed Twitter app to
offer a single sign-on experience. If TwitterCore is unable to access the authentication token through
the Twitter app, it falls back to using a web view to finish the OAuth process.

The simplest way to authenticate a user is using`TwitterLoginButton`. Inside your layout, add
a Login button with the following code:

```xml

   <com.twitter.sdk.android.core.identity.TwitterLoginButton
        android:id="@+id/login_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

This will render a button that looks like:

![](http://twitter.github.io/twitter-kit-android/images/signin-with-twitter.png)


# Log In Button

In the Activity or Fragment that displays the button, you need to create and
attach a `Callback` to the Login Button.

```java

   import com.twitter.sdk.android.core.Callback;
   import com.twitter.sdk.android.core.Result;
   import com.twitter.sdk.android.core.TwitterException;
   import com.twitter.sdk.android.core.TwitterSession;
   import com.twitter.sdk.android.core.identity.TwitterLoginButton;
   ...

   loginButton = (TwitterLoginButton) findViewById(R.id.login_button);
   loginButton.setCallback(new Callback<TwitterSession>() {
      @Override
      public void success(Result<TwitterSession> result) {
          // Do something with result, which provides a TwitterSession for making API calls
      }

      @Override
      public void failure(TwitterException exception) {
          // Do something on failure
      }
   });
```

## Pass the Activity's Result Back to the Button


Next, pass the result of the authentication Activity back to the button:

```java

   @Override
   protected void onActivityResult(int requestCode, int resultCode, Intent data) {
       super.onActivityResult(requestCode, resultCode, data);

       // Pass the activity result to the login button.
       loginButton.onActivityResult(requestCode, resultCode, data);
   }
   ```

If using the TwitterLoginButton in a Fragment, use the following steps instead. Inside the Activity hosting the Fragment, pass the result from the Activity to the Fragment.

```java

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        // Pass the activity result to the fragment, which will then pass the result to the login
        // button.
        Fragment fragment = getFragmentManager().findFragmentById(R.id.your_fragment_id);
        if (fragment != null) {
            fragment.onActivityResult(requestCode, resultCode, data);
        }
    }
```

## TwitterSession

If login completes successfully, a `TwitterSession` is provided in the success result. This
TwitterSession will contain a token, secret, username, and user ID of the user and becomes the
active session and is automatically persisted. If you need to retrieve the TwitterSession at a later
time, you may do so using the `SessionManager`.

```java

    TwitterSession session = TwitterCore.getInstance().getSessionManager().getActiveSession();
    TwitterAuthToken authToken = session.getAuthToken();
    String token = authToken.token;
    String secret = authToken.secret;
```

# Request User Email Address

Before using this feature, ensure that "Request email addresses from users" is checked for your Twitter app [Twitter Develop Console](https://apps.twitter.com). To request a user's email, call the `TwitterAuthClient#requestEmail` method, passing in a
valid `TwitterSession` and `Callback`.

```java

    TwitterAuthClient authClient = new TwitterAuthClient();
    authClient.requestEmail(session, new Callback<String>() {
        @Override
        public void success(Result<String> result) {
            // Do something with the result, which provides the email address
        }

        @Override
        public void failure(TwitterException exception) {
          // Do something on failure
        }
    });
```

If the email address is available, the success method is called with the email address in the result. It is not guaranteed you will get an email address. For example, if someone signed up for Twitter with a phone number instead of an email address, the email field may be empty. When this happens, the failure method will be called because there is no email address available.



# Guest Authentication

Twitter supports two authentication types for a logged out Twitter experience. [Application authentication](https://dev.twitter.com/oauth/application-only) allows an app to issue API requests on its behalf instead of the user’s; it is limited to requests that do not require a user context. This means, for example, that you *cannot* Tweet with application authentication, but you *can* get a user's last Tweet. See [Access Twitter’s REST API](./Access-Twitter’s-REST-API.html). Guest authentication is an extension to application authentication, but there are two major differences:

## 1. Rate Limits

When using application authentication, rate limits are determined globally for the entire app. For example, an app can make 300 requests every 15 minutes against the [GET statuses/user_timeline](https://developer.twitter.com/en/docs/tweets/timelines/api-reference/get-statuses-user_timeline) endpoint. This means that if your mobile app is being concurrently used by 100 users – those 300 requests are *shared* by 100 users. On the other hand, guest authentication rate limits scale – *each* of those 100 users will now be allocated 180 requests every 15 minutes.

Internally, Twitter Kit calls a number of endpoints, notably:

- [GET lists/statuses](https://developer.twitter.com/en/docs/accounts-and-users/create-manage-lists/api-reference/get-lists-statuses)
- [GET search/tweets](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets)
- [GET statuses/user_timeline](https://developer.twitter.com/en/docs/tweets/timelines/api-reference/get-statuses-user_timeline)
- [GET statuses/show/:id](https://developer.twitter.com/en/docs/tweets/post-and-engage/api-reference/get-statuses-show-id)
- [GET statuses/lookup](https://developer.twitter.com/en/docs/tweets/post-and-engage/api-reference/get-statuses-lookup)
- [GET collections/entries](https://developer.twitter.com/en/docs/tweets/curate-a-collection/api-reference/get-collections-entries)

These endpoints are rate limited to 180 requests per 15 minutes under guest authentication, the exception being GET collection/entries which is limited to 1000 requests per 15 minutes.

## 2. Token Expiration

User tokens do not expire, but those generated by guest authentication do. However, Twitter Kit will automatically handle guest token refresh.
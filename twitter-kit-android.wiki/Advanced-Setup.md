# Use Existing Twitter API Keys


You should use API keys from a Twitter app generated on [Twitter Developer Console](https://apps.twitter.com).

To use existing Twitter API Keys:

* Go to your Twitter application's settings at [Twitter's developer site](http://dev.twitter.com/apps).
* Find your application and go to the permissions tab.
* Select the appropriate permissions for your needs (e.g. "Read and write")
* If you are using login, add a placeholder URL in the Callback URL field (eg. "http://placeholder.com").
* Click update settings.

Although the callback URL will not be requested by Twitter Kit in your app, it __be set to a valid URL for the app to work with the SDK__.

![](http://twitter.github.io/twitter-kit-android/images/callback-url.png)
 

## Allow Log In with Twitter

If you wish to use Log In with Twitter, be sure that __Allow this application to be used to Sign in to Twitter__ is checked:

![](http://twitter.github.io/twitter-kit-android/images/signin-enabled.png)

## Additional Permissions

If your app requires writing Tweets or accessing DM's you will need to enable additional permissions for your app.

Changes to the application permission model will only take effect in access tokens obtained after the permission model change is saved. You will need to re-negotiate existing access tokens to alter the permission level associated with each of your application's users.

![](http://twitter.github.io/twitter-kit-android/images/app-permissions.png)
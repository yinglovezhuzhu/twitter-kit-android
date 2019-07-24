TweetUi provides Tweet views to render Tweets in your app via code or in XML.

**Features**

* Two variants to choose from:

    * TweetView: Standard Tweet view with photo forward.
    * CompactTweetView: Tweet view with cropped photo, for use in list displays.
* Tweet action buttons (favorite, share) can optionally be enabled.
* Tap Tweet view to open Tweet permalink using default url intent.
* Tweet views automatically use `user authentication <Log-In-with-Twitter>`__ if available or `guest authentication <Log-In-with-Twitter#guest-authentication>`__.

Tweet Views
===========

TweetUi provides the :code:`TweetView` and :code:`CompactTweetView` views for rendering Tweets. The view constructors take a context, the Tweet to be rendered, and an optional `Styling`_.

Tweets can be requested through the TwitterCore `Access Twitter’s REST API <Access-Twitter’s-REST-API>`__ or :code:`TweetUtils` which will cache recent requests. Here is an example using :code:`TweetUtils` to load a Tweet and construct a Tweet view in the success callback.

.. code-block:: java

    // EmbeddedTweetActivity
    import com.twitter.sdk.android.core.Callback;
    import com.twitter.sdk.android.core.models.Tweet;
    import com.twitter.sdk.android.core.TwitterException;
    import com.twitter.sdk.android.tweetui.TweetUtils;
    import com.twitter.sdk.android.tweetui.TweetView;
    ...

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_embedded_tweet);

        final LinearLayout myLayout
                = (LinearLayout) findViewById(R.id.my_tweet_layout);

        final long tweetId = 510908133917487104L;
        TweetUtils.loadTweet(tweetId, new Callback<Tweet>() {
            @Override
            public void success(Result<Tweet> result) {
                myLayout.addView(new TweetView(EmbeddedTweetsActivity.this, tweet));
            }

            @Override
            public void failure(TwitterException exception) {
                // Toast.makeText(...).show();
            }
        });
    }

.. figure:: http://twitter.github.io/twitter-kit-android/images/tweetview.png
   :width: 320px
   :alt: TweetView with a photo

XML
---

The TweetView and CompactTweetView can be inflated from an XML layout as well. For this usage, the :code:`tw__tweet_id` attribute **must** specify the Tweet id and the Tweet will be loaded and rendered upon inflation.

.. code-block:: xml

    <!--my_tweet_activity.xml-->

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:twittersdk="http://schemas.android.com/apk/res-auto">

        <com.twitter.sdk.android.tweetui.TweetView
            android:id="@+id/bike_tweet"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            twittersdk:tw__tweet_id="510908133917487104"/>

    </FrameLayout>

.. note:: You **must** use the layout parameters :code:`android:layout_width="match_parent"` and :code:`layout_height="wrap_content"`.

.. _tweet_actions:

Tweet Actions
=============

Tweet views have the option to show Tweet action buttons (favorite, share). By default, actions are disabled but may be enabled by setting :code:`tw__tweet_actions_enabled` property or calling :code:`BaseTweetView#setTweetActionsEnabled(boolean)`. For convenience :code:`tw__TweetLightWithActionsStyle` and :code:`tw__TweetDarkWithActionsStyle` styles are provided for enabling actions (See: `Styling`_). The favorite action uses the user session to perform the favorite. If no user session is available, the action will do nothing. By setting an action callback which handles TwitterAuthException failures, you could launch your login flow so that when the user returns to the Tweet view, the favorite action succeeds.

Here is an example that shows enabling actions and setting an action callback that will start some login activity if a user session isn't present.

.. code-block:: java

    // EmbeddedTweetActivity.java
    import com.twitter.sdk.android.tweetui.TweetView;
    ...

    // launch the login activity when a guest user tries to favorite a Tweet
    final Callback<Tweet> actionCallback = new Callback<Tweet>() {
        @Override
        public void success(Result<Tweet> result) {
           // Intentionally blank
        }

        @Override
        public void failure(TwitterException exception) {
            if (exception instanceof TwitterAuthException) {
                // launch custom login flow
                startActivity(LoginActivity.class);
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_embedded_tweet);

        final LinearLayout myLayout
                = (LinearLayout) findViewById(R.id.my_tweet_layout);

        final long tweetId = 510908133917487104L;
        TweetUtils.loadTweet(tweetId, new Callback<Tweet>() {
            @Override
            public void success(Result<Tweet> result) {
                final TweetView tweetView = new TweetView(EmbeddedTweetsActivity.this, result.data,
                        R.style.tw__TweetDarkWithActionsStyle)
                tweetView.setOnActionCallback(actionCallback)
                myLayout.addView(tweetView);
            }

            @Override
            public void failure(TwitterException exception) {
                // Toast.makeText(...).show();
            }
        });
    }

Styling
=======

TweetUi provides four Android styles :code:`tw__TweetLightStyle`, :code:`tw__TweetLightWithActionsStyle`, :code:`tw__TweetDarkStyle`, and :code:`tw__TweetDarkWithActionsStyle`. The light style is used by default.

The desired style may be specified in the view's constructor.

.. code-block:: java

    // EmbeddedTweetActivity.java
    import com.twitter.sdk.android.tweetui.TweetView;
    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_embedded_tweet);

        final LinearLayout myLayout
                = (LinearLayout) findViewById(R.id.my_tweet_layout);

        final long tweetId = 510908133917487104L;
        TweetUtils.loadTweet(tweetId, new Callback<Tweet>() {
            @Override
            public void success(Result<Tweet> result) {
                myLayout.addView(new TweetView(EmbeddedTweetsActivity.this, result.data,
                        R.style.tw__TweetDarkWithActionsStyle));
            }

            @Override
            public void failure(TwitterException exception) {
                // Toast.makeText(...).show();
            }
        });
    }

.. figure:: http://twitter.github.io/twitter-kit-android/images/tweetview-dark-actions.png
   :width: 320px
   :alt: XML TweetView

   A dark style TweetView with Tweet actions enabled.

For Tweet views in XML, the style may be set by the standard :code:`@style` attribute.

.. code-block:: xml

    <!--my_tweet_activity.xml-->

    <com.twitter.sdk.android.tweetui.TweetView
        android:id="@+id/city_hall_tweet"
        style="@style/tw__TweetDarkWithActionsStyle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        twittersdk:tw__tweet_id="521061104445693952"/>

Custom Styling
--------------

:code:`TweetView` and :code:`CompactTweetView` respect custom style attributes to change the way a Tweet looks:

* tw__container_bg_color - the background color of the Tweet
* tw__primary_text_color - the Tweet text color and author name color
* tw__action_color - the color of links in Tweet text
* tw__tweet_actions_enabled - show or hide Tweet actions

The provided styles set these custom attributes to fixed values. To customize the style of a Tweet view further, the easiest approach is to extend one of the existing styles.

.. code-block:: xml

    <!--styles.xml-->

    <style name="CustomLightTweetStyle" parent="tw__TweetDarkStyle">
        <item name="tw__primary_text_color">@color/custom_color_1</item>
        <item name="tw__action_color">@color/custom_color_2</item>
    </style>

Alternately, you may create a theme or a style that sets one or more of the custom attributes. A theme can be applied to the application or activities while a style can be applied to individual views.

.. code-block:: xml

    <!--styles.xml-->

    <style name="CustomStyle">
        <item name="tw__container_bg_color">@color/custom_color_1</item>
        <item name="tw__primary_text_color">@color/custom_color_2</item>
    </style>

    <style name="CustomTheme" parent="android:Theme.Holo.Light">
        <item name="tw__container_bg_color">@color/custom_color_1</item>
    </style>

If a Tweet view renders and one of the custom style attributes is not set, the value defaults to the :code:`tw__TweetLightStyle`'s value.
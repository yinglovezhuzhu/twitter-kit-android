TwitterCore provides a :code:`TwitterApiClient` for making authenticated Twitter API requests. It currently has support for the following:

* Statuses (Tweets)
    * mentionsTimeline
    * userTimeline
    * homeTimeline
    * retweetsOfMe
    * show
    * lookup
    * update
    * retweet
    * unretweet
    * destroy
* Favorites
    * list
    * create
    * destroy
* Search
    * tweets
* Lists
    * statuses
* Collections
    * entries

Your app must have acquired a Session in order to use the TwitterApiClient. A TwitterSession can be acquired by having a user `login with Twitter <Log-In-with-Twitter>`__. To make requests without requiring a user to login, `guest authentication <Log-In-with-Twitter#guest-authentication>`__ can be used. Note, guest authentication may only be used to make API requests that do not require a user context. For example, you cannot Tweet with a GuestSession.

Once you are authenticated you can start using the TwitterApiClient to make API Requests.

.. _tweet_api:

Tweets
======

.. code-block:: java

    TwitterApiClient twitterApiClient = TwitterCore.getInstance().getApiClient();
    StatusesService statusesService = twitterApiClient.getStatusesService();
    Call<Tweet> call = statusesService.show(524971209851543553L, null, null, null);
    call.enqueue(new Callback<Tweet>() {
        @Override
        public void success(Result<Tweet> result) {
            //Do something with result
        }

        public void failure(TwitterException exception) {
            //Do something on failure
        }
    });

Extensions
==========

TwitterApiClient is fully extensible and can be extended to add additional Twitter authenticated endpoints. In addition, TwitterApiClient can be constructed with user defined HTTP client, which enables developers to set HTTP interceptors, custom timeouts, and other optional params defined in OkHttpClient.

Extending TwitterApiClient
--------------------------

Extending TwitterApiClient to add additional Twitter authenticated endpoints.

.. code-block:: java

    import com.twitter.sdk.android.core.Callback;
    import com.twitter.sdk.android.core.Result;
    import com.twitter.sdk.android.core.TwitterApiClient;
    import com.twitter.sdk.android.core.TwitterException;
    import com.twitter.sdk.android.core.TwitterSession;
    import com.twitter.sdk.android.core.models.User;
    import retrofit.http.Query;

    class MyTwitterApiClient extends TwitterApiClient {
        public MyTwitterApiClient(TwitterSession session) {
            super(session);
        }

        /**
         * Provide CustomService with defined endpoints
         */
        public CustomService getCustomService() {
            return getService(CustomService.class);
        }
    }

    // example users/show service endpoint
    interface CustomService {
        @GET("/1.1/users/show.json")
        Call<User> void show(@Query("user_id") long id);
    }

Twitter Kit uses Retrofit to convert interfaces into authenticated representations of our endpoints. Any additional endpoints added through the extensible example need to adhere to the format required by Retrofit.


Log Requests and Responses
--------------------------

To demonstrate an example of creating a custom HTTP client, we will setup OkHttp's :code:`HttpLoggingInterceptor` for logging requests and responses.

.. note:: HttpLoggingInterceptor is not provided in Twitter Kit.

.. code-block:: java

    import okhttp3.OkHttpClient;
    import okhttp3.logging.HttpLoggingInterceptor;

    import com.twitter.sdk.android.Twitter;
    import com.twitter.sdk.android.core.TwitterApiClient;
    import com.twitter.sdk.android.core.TwitterAuthConfig;
    import com.twitter.sdk.android.core.TwitterCore;
    import com.twitter.sdk.android.core.TwitterSession;

    class SampleApplication extends Application {

         @Override
        public void onCreate() {
            ...

            final TwitterSession activeSession = TwitterCore.getInstance()
                        .getSessionManager().getActiveSession();

            // example of custom OkHttpClient with logging HttpLoggingInterceptor
            final HttpLoggingInterceptor loggingInterceptor = new HttpLoggingInterceptor();
            loggingInterceptor.setLevel(HttpLoggingInterceptor.Level.BASIC);
            final OkHttpClient customClient = new OkHttpClient.Builder()
                    .addInterceptor(loggingInterceptor).build();

            // pass custom OkHttpClient into TwitterApiClient and add to TwitterCore
            final TwitterApiClient customApiClient;
            if (activeSession != null) {
                customApiClient = new TwitterApiClient(activeSession, customClient);
                TwitterCore.getInstance().addApiClient(activeSession, customApiClient);
            } else {
                customApiClient = new TwitterApiClient(customClient);
                TwitterCore.getInstance().addGuestApiClient(customApiClient);
            }
        }

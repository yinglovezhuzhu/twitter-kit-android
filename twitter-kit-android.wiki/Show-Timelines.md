TweetUi provides Timelines for adding scrollable timelines anywhere in your app.

**Types**

* `User Timeline <https://dev.twitter.com/rest/reference/get/statuses/user_timeline>`_
* `Search Timeline <https://dev.twitter.com/rest/reference/get/search/tweets>`_
* `Collection Timeline <https://dev.twitter.com/rest/reference/get/collections/entries>`_
* `List Timeline <https://dev.twitter.com/rest/reference/get/lists/statuses>`_

**Features**

* Automatic guest authentication
* Adapter loads previous items
* Adapter :code:`refresh` method for implementing pull to refresh

Timelines
=========

To show a timeline in your app, first create a RecyclerView or ListView in an Activity or Fragment. Next, choose a :code:`Timeline` instance (:code:`UserTimeline`, :code:`SearchTimeline`, :code:`CollectionTimeline`, :code:`TwitterListTimeline`, or :code:`FixedTweetTimeline`) and configure it using the Builder. Finally, build a :code:`TweetTimelineRecyclerViewAdapter` or a :code:`TweetTimelineListAdapter` as the adapter to your RecyclerView or ListView adapter respectively and provide it with the :code:`Timeline` instance. :code:`TweetTimelineRecyclerViewAdapter` and :code:`TweetTimelineListAdapter` both accept any :code:`Timeline` and handle loading older Tweets and recycling views.

Timelines use guest authentication automatically so no auth setup is needed. More advanced Builder options are documented below.

Examples
========

:code:`UserTimeline` which shows the `@twitterdev <https://twitter.com/twitterdev>`_ user's timeline of Tweets:

.. code-block:: java

    UserTimeline userTimeline = new UserTimeline.Builder().screenName("twitterdev").build();

:code:`SearchTimeline` which shows Tweets matching the query `"#twitterflock" <https://twitter.com/search?q=%23twitterflock&src=typd&vertical=default&lang=en>`_:

.. code-block:: java

    SearchTimeline searchTimeline = new SearchTimeline.Builder()
      .query("#twitterflock")
      .build();

:code:`CollectionTimeline` for the `National Parks Tweets <https://twitter.com/TwitterDev/timelines/539487832448843776>`__ example collection of Tweets:

.. code-block:: java

  CollectionTimeline timeline = new CollectionTimeline.Builder()
    .id(539487832448843776)
    .build();

:code:`TwitterListTimeline` for the @twitterdev `national-parks <https://twitter.com/TwitterDev/lists/national-parks>`_ list can be created with the owner screen name (or user id) and list name:

.. code-block:: java

    final TwitterListTimeline timeline = new TwitterListTimeline.Builder()
        .slugWithOwnerScreenName("national-parks", "twitterdev")
        .build();

:code:`FixedTweetTimeline` which shows a fixed set of Tweets:

.. code-block:: java

    final FixedTweetTimeline timeline = new FixedTweetTimeline.Builder()
        .setTweets(listOfTweets)
        .build();

Recycler View
-------------
Here is an example Activity setting up a RecyclerView using :code:`SearchTimeline`:

.. code-block:: java

    import com.twitter.sdk.android.tweetui.SearchTimeline;
    import com.twitter.sdk.android.tweetui.TweetTimelineRecyclerViewAdapter;

    public class TimelineRecyclerViewActivity extends AppCompatActivity {

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.tweetui_timeline_recyclerview);

            final RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recycler_view);

            recyclerView.setLayoutManager(new LinearLayoutManager(this));

            final SearchTimeline searchTimeline = new SearchTimeline.Builder()
                    .query("#hiking")
                    .maxItemsPerRequest(50)
                    .build();

            final TweetTimelineRecyclerViewAdapter adapter =
                    new TweetTimelineRecyclerViewAdapter.Builder(this)
                            .setTimeline(searchTimeline)
                            .setViewStyle(R.style.tw__TweetLightWithActionsStyle)
                            .build();

            recyclerView.setAdapter(adapter);
        }
    }

ListActivity
------------

Here is an example Activity setting up a :code:`UserTimeline`:

.. code-block:: java

    import com.twitter.sdk.android.tweetui.TweetTimelineListAdapter;
    import com.twitter.sdk.android.tweetui.UserTimeline;

    public class TimelineActivity extends ListActivity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.timeline);

            final UserTimeline userTimeline = new UserTimeline.Builder()
                .screenName("twitterdev")
                .build();
            final TweetTimelineListAdapter adapter = new TweetTimelineListAdapter.Builder(this)
                .setTimeline(userTimeline)
                .build();
            setListAdapter(adapter);
        }
    }

and the corresponding XML layout :code:`timeline.xml`, showing attribute values without resource references for clarity:

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView android:id="@id/android:empty"
                  android:layout_width="match_parent"
                  android:layout_height="match_parent"
                  android:gravity="center_horizontal|center_vertical"
                  android:text="No Tweets"/>

        <ListView android:id="@id/android:list"
                  android:layout_width="match_parent"
                  android:layout_height="0dp"
                  android:layout_weight="1"
                  android:divider="#e1e8ed"
                  android:dividerHeight="1dp"
                  android:drawSelectorOnTop="false"/>
    </LinearLayout>

.. figure:: http://twitter.github.io/twitter-kit-android/images/user-timeline.png
   :width: 320px
   :alt: User Timeline

As a user scrolls down previous Tweets will be loaded into the ListView. Note, TimelineListAdapters currently load a maximum of 200 items.

.. _collection-timeline:

Collection Timeline
===================

The Twitter `collections documentation <https://dev.twitter.com/rest/collections>`_ describes collection timelines and shows how they can be created and managed. One approach is through TweetDeck as described in `this post <https://support.twitter.com/articles/20170322#>`_.

After you've created a collection, view it on twitter.com, and get the id from the address bar. The example `National Parks Tweets <https://twitter.com/twitterdev/timelines/539487832448843776>`__ collection has id :code:`539487832448843776`. Create a :code:`CollectionTimeline` and provide it to a :code:`TweetTimelineListAdapter` set on a ListView.

.. code-block:: java

    import com.twitter.sdk.android.tweetui.TweetTimelineListAdapter;
    import com.twitter.sdk.android.tweetui.CollectionTimeline;

    public class TimelineActivity extends ListActivity {

        final Callback<Tweet> actionCallback = ...

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.timeline);

            // Collection "National Parks Tweets"
            final CollectionTimeline timeline = new CollectionTimeline.Builder()
                .id(539487832448843776L)
                .build();
            final TweetTimelineListAdapter adapter = new TweetTimelineListAdapter.Builder(this)
                .setTimeline(timeline)
                .setViewStyle(R.style.tw__TweetLightWithActionsStyle)
                .setOnActionCallback(actionCallback)
                .build();
            setListAdapter(adapter);
        }
    }

.. figure:: http://twitter.github.io/twitter-kit-android/images/collection-timeline.png
   :width: 320px
   :alt: Collection Timeline

Tweets added to the collection (via TweetDeck or other services) will appear in this CollectionTimeline.

To implement a more advanced pull to refresh adapter, see the section on `Pull to Refresh`_.

.. _filter_timeline:

Filter Timeline
===============

Twitter Kit provides functionality to filter the Tweets displayed in your app, according to rules you provide. You can use this functionality to prevent showing Tweets with profane words, hide Tweets that link to blacklisted URLs, or block particular user's Tweets from appearing in your app. The rules can be managed in a standard JSON configuration file, and used on any timeline in Twitter Kit for Android and iOS.

The :code:`TweetTimelineListAdapter` supports the ability specify a :code:`TweetFilter` to filter timeline content. :code:`BasicTimelineFilter` which implements the :code:`TweetFilter` interface supports filtering of Tweet by hashtags, urls, handles, and keywords.

.. code-block:: java

    public class SearchTimelineFragment extends ListActivity {

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            final List<String> handles = Arrays.asList("ericfrohnhoefer", "benward", "vam_si");
            final FilterValues filterValues = new FilterValues(null, null, handles, null); // or load from JSON, XML, etc
            final TimelineFilter timelineFilter = new BasicTimelineFilter(filterValues, Locale.ENGLISH);

            final SearchTimeline searchTimeline = new SearchTimeline.Builder()
                .query("#twitterflock")
                .languageCode(Locale.ENGLISH.getLanguage())
                .build();
            final TweetTimelineListAdapter adapter = new TweetTimelineListAdapter.Builder(this)
                .setTimeline(searchTimeline)
                .setTimelineFilter(timelineFilter)
                .build();
            setListAdapter(adapter);
        }

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                Bundle savedInstanceState) {
            return inflater.inflate(R.layout.timeline, container, false);
        }
    }

Using the :code:`BasicTimelineFilter` one can specify four different filters using the :code:`FilterValues` object.

+--------------------+-------------------------------------------------------------------------------------------------------+
| Filter Value       | Description                                                                                           |
+====================+=======================================================================================================+
| keywords           | Removes Tweets containing specified keywords in Tweet text. Uses localized case-insensitive matching. |
+--------------------+-------------------------------------------------------------------------------------------------------+
| hashtags           | Removes Tweets containing specified hashtags. Uses localized case-insensitive matching.               |
+--------------------+-------------------------------------------------------------------------------------------------------+
| handles            | Removes Tweets from specified user or replies to specified user. Uses case-insensitive matching.      |
+--------------------+-------------------------------------------------------------------------------------------------------+
| urls               | Removes Tweet containing URLs from specified domain. Supports internationalized domain names.         |
+--------------------+-------------------------------------------------------------------------------------------------------+

You can easily load your filter settings using a JSON configuration file and GSON, like so:

.. code-block:: json

    {
      "keywords": [
        "dummy"
      ],
      "hashtags": [
        "cookies"
      ],
      "handles": [
        "benward",
        "vam_si",
        "ericfrohnhoefer"
      ],
      "urls": [
        "example.com"
      ]
    }

.. code-block:: java

    final InputStream inputStream = getResources().openRawResource(R.raw.filter_values);
    final JsonReader reader = new JsonReader(new InputStreamReader(inputStream));
    final FilterValues filterValues = new Gson().fromJson(reader, FilterValues.class);

The above example loads the configuration from a raw resource. However, you can store the configuration file locally, or pull from a remote server to deliver over-the-air updates to your filter configuration. You can use the same configuration file for Twitter Kit on both Android and iOS.

.. _pull_to_refresh:

Pull to Refresh
===============

The TweetTimelineListAdapter supports a refresh method which loads the latest Tweets from its timeline and replaces existing list items. You might use this to implement pull to refresh.

Here is how refresh might be called if using a :code:`SwipeRefreshLayout`.

.. code-block:: java

    import android.support.v4.widget.SwipeRefreshLayout;
    import com.twitter.sdk.android.core.Callback;
    import com.twitter.sdk.android.core.Result;
    import com.twitter.sdk.android.core.TwitterException;
    import com.twitter.sdk.android.core.models.Tweet;
    import com.twitter.sdk.android.tweetui.SearchTimeline;
    import com.twitter.sdk.android.tweetui.TimelineResult;
    import com.twitter.sdk.android.tweetui.TweetTimelineListAdapter;
    ...

    // inside onCreate
    final SwipeRefreshLayout swipeLayout = (SwipeRefreshLayout) findViewById(R.id.swipe_layout);
    final SearchTimeline timeline = new SearchTimeline.Builder()
                .query("#twitter")
                .build();
    final TweetTimelineListAdapter adapter = new TweetTimelineListAdapter.Builder(this)
                .setTimeline(timeline)
                .build();
    setListAdapter(adapter);

    swipeLayout.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
        @Override
        public void onRefresh() {
            swipeLayout.setRefreshing(true);
            adapter.refresh(new Callback<TimelineResult<Tweet>>() {
                @Override
                public void success(Result<TimelineResult<Tweet>> result) {
                    swipeLayout.setRefreshing(false);
                }

                @Override
                public void failure(TwitterException exception) {
                    // Toast or some other action
                }
            });
        }
    });


.. _timeline_builders:

Timeline Builders
=================

User Timeline
-------------

User timelines show the Tweet timeline of an individual Twitter user. The :code:`UserTimeline.Builder` provides the following methods:

+--------------------+--------------------------------------------------+
| Builder Method     | Description                                      |
+====================+==================================================+
| userId             | The ID of the user to show tweets for.           |
+--------------------+--------------------------------------------------+
| screenName         | The screen name of the user to show tweets for.  |
+--------------------+--------------------------------------------------+
| maxItemsPerRequest | Max number of items to return per request.       |
+--------------------+--------------------------------------------------+
| includeReplies     | Whether to include replies. Defaults to false.   |
+--------------------+--------------------------------------------------+
| includeRetweets    | Whether to include retweets. Defaults to true.   |
+--------------------+--------------------------------------------------+

Search Timeline
---------------

Search timelines show search results with recent Tweets (~ 1 week) matching a given search query. Check the `Search API <https://dev.twitter.com/rest/public/search>`_ for information about the advanced search query parameters that can be used in your search query string.
The :code:`SearchTimeline.Builder` provides the following methods:

+--------------------+-------------------------------------------------------------------------------------------------------------+
| Builder Method     | Description                                                                                                 |
+====================+=============================================================================================================+
| query              | The search query for Tweets.                                                                                |
+--------------------+-------------------------------------------------------------------------------------------------------------+
| languageCode       | Restricts Tweets to the given `ISO 639-1 <https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes>`_ language.|
+--------------------+-------------------------------------------------------------------------------------------------------------+
| maxItemsPerRequest | Max number of items to return per request.                                                                  |
+--------------------+-------------------------------------------------------------------------------------------------------------+
| resultType         | Allows one to choose if the result set will be represented by recent or popular Tweets, or a mix of both.   |
+--------------------+-------------------------------------------------------------------------------------------------------------+
| untilDate          | Returns tweets generated before the given date. Date should be formatted as YYYY-MM-DD.                     |
+--------------------+-------------------------------------------------------------------------------------------------------------+

Collection Timeline
-------------------

Collection timelines show the Tweets of a collection, a user managed collection of Tweets selected either by hand or programmatically.
The :code:`CollectionTimeline.Builder` provides the following methods:

+--------------------+--------------------------------------------------+
| Builder Method     | Description                                      |
+====================+==================================================+
| id                 | The ID of the collection to show tweets for.     |
+--------------------+--------------------------------------------------+
| maxItemsPerRequest | Max number of items to return per request.       |
+--------------------+--------------------------------------------------+


Twitter List Timeline
---------------------

Twitter List timelines show a user's Twitter List, which collects Tweets authored by members of the list. The :code:`TwitterListTimeline.Builder` provides the following methods:

+-------------------------+-----------------------------------------------------------------+
| Builder Method          | Description                                                     |
+=========================+=================================================================+
| id                      | Specifies the list by ID.                                       |
+-------------------------+-----------------------------------------------------------------+
| slugWithOwnerId         | Specifies the list by the owner User ID and list name.          |
+-------------------------+-----------------------------------------------------------------+
| slugWithOwnerScreenName | Specifies the list by owner screen name and list name.          |
+-------------------------+-----------------------------------------------------------------+
| includeRetweets         | Whether retweets should be included; defaults to true.          |
+-------------------------+-----------------------------------------------------------------+
| maxItemsPerRequest      | Max number of items to return per request.                      |
+-------------------------+-----------------------------------------------------------------+

Fixed Tweet Timeline
--------------------

Fixed Tweet timelines show a fixed set of Tweets. The :code:`FixedTweetTimeline.Builder` provides the following methods:

+--------------------+--------------------------------------------------+
| Builder Method     | Description                                      |
+====================+==================================================+
| setTweets          | Fixed set of Tweets provided by the timeline.    |
+--------------------+--------------------------------------------------+

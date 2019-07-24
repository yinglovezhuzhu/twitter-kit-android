Twitter
=======

3.2.0 Nov 13, 2017
-------------------
[Breaking Change - requires Java 8 language features enabled]

* Fixed issue that caused startup delays
* Moved build to Java 8

3.1.1 August 10, 2017
---------------------

* Modified TweetTimelineRecyclerViewAdapter to allow user to override the ViewHolder
* Send broadcast when Tweet compose is cancelled

3.1.0 July 26, 2017
-------------------

* Updated Tweet UI and Twitter Core dependencies

3.0.0 May 25, 2017
------------------

* Removed all static helpers from Twitter class
* Updated Tweet UI and Twitter Core dependencies
* For more details checkout `Upgrade to Twitter Kit 3.0 <Upgrade-to-Twitter-Kit-3.0>`__

2.3.2 February 23, 2017
-----------------------

* Updated Tweet UI and Twitter Core dependencies

2.3.1 December 22, 2016
-----------------------

* Updated Tweet UI and Twitter Core dependencies

2.3.0 December 8, 2016
----------------------

* Added support for timeline filtering
* Updated Tweet UI and Twitter Core dependencies

2.2.0 November 10, 2016
-----------------------

* Added support for quote Tweet display
* Added support for user defined OkHttpClient
* Updated Tweet UI and Twitter Core dependencies

2.1.1 October 10, 2016
----------------------

* Fixed UnsupportedOperationException when rendering multiple photos API 17 and below
* Updated Tweet UI dependency to version 2.1.1
* Added multi-photo support for inline views and the fullscreen gallery view
* Fixed IllegalArgumentException in GuestAuthenticator
* Updated Twitter dependencies

2.0.1 September 22, 2016
------------------------

* Updated ProGuard rules for OkHttp3 and Retrofit2
* Added translations for Twitter Composer
* Removed test-only pseudo locales from translations
* Moved TwitterCollection from internal package to models
* Minor bug fixes

2.0.0 August 11, 2016
---------------------

* Removed Digits dependency
* Dropped support for API versions before API 14 (ICS)
* Updated Twitter Core, Tweet UI, and Tweet Composer dependencies

1.14.1 July 01, 2016
--------------------

* Added support for additional result types for a SearchTimeline

1.14.0 June 23, 2016
--------------------

* Added support for Vine in Tweets
* Enabled extended Tweet display. Display full Tweet text when total length of a Tweet exceeds 140 characters

1.13.3 June 15, 2016
--------------------

* Updated Twitter Core Dependency

1.13.2 June 1, 2016
-------------------

* Updated Fabric dependency

1.13.1 April 27, 2016
---------------------

* Fixed security issue where certificate pinning was not happening for some requests
* Added HLS playback support. Removed WebM playback support
* Added loading and buffering spinners to video player
* Only show play button and media badge if playback is supported

1.13.0 March 4, 2016
--------------------

* Added click listeners on Tweet views for URL and media clicks
* Removed Verisign Class 3 Certificate from pinning list
* Fixed JavaDocs

1.12.1 February 4, 2016
-----------------------

* Fixed issue retrieving auth token when using OkHttp 2.3+
* Added gif or duration badge to media view

1.12.0 January 12, 2016
-----------------------

* Added basic photo viewer
* Added custom controls for video playback
* Enabled looping for animated GIFs

1.11.0 December 4, 2015
-----------------------

* Added support for video and animated GIF playback

1.10.0 November 12, 2015
------------------------

* Change TweetView favorite stars to like hearts
* Updated translations

1.9.1 October 29, 2015
----------------------

* Fixed incorrect large Twitter logo in TweetViews

1.9.0: October 19, 2015
-----------------------

* Removed usage of deprecated Apache HTTP Client constants
* Moved GuestCallback from internal package
* Moved AspectRatioImageView from tweet-ui to twitter-core
* Moved tweet-ui UserUtils from tweet-ui to twitter-core

1.8.0: August 31, 2015
----------------------

* Bump to Digits version 1.8.0

1.7.2: August 20, 2015
----------------------

* Fixed bug in guest auth queue which cached application-only auth fallback tokens indefinitely. Change queue to disallow and clear application-only auth tokens
* Removed logInGuest fallback to application-only auth so logInGuest consistently provides an AppSession with a GuestAuthToken. Previously, logInGuest provided an AppSession which could fall back to a non-expiring app auth token when guest token requests failed
* Fixed bug in TwitterApiException’s TwitterRateLimit.getRemainingTime()

1.7.1: August 12, 2015
----------------------

* Bump to Digits version 1.7.1

1.7.0: August 5, 2015
---------------------

* Added Tweet actions to Tweet views
* Added TweetTimelineListAdapter Builder to support setting style and enabling actions
* Added FixedTweetTimeline for fixed sets of provided Tweets
* Deprecated TweetViewAdapter and TweetViewFetchAdapter. To Upgrade:
* If required, load Tweets by id with TweetUtils.loadTweets
* Build a FixedTweetTimeline with the set of Tweets to display, pass the timeline to the TweetTimelineListAdapter, and set the adapter on your ListView (consistent with other Timelines)

1.6.1: July 16, 2015
--------------------

* Moved AuthRequestQueue to twitter-core
* Fixed critical issue where Twitter sessions are lost when using ProGuard
* Added support for Twitter Single Sign-on with the Twitter Android Dogfood App
* Added unretweet endpoint to StatusesService
* Switched CollectionService from the beta collections endpoint to the public collections endpoint
* Changed TweetUtils loadTweet(s) to take a TwitterCore Callback. Deprecate those taking a LoadCallback
* Changed TweetViewFetchAdapter setTweetIds to take a TwitterCore Callback. Deprecate the method taking a LoadCallback

1.6.0: June 10, 2015
--------------------

* Improved security of WebView used by OAuthActivity by disabling form data saving
* Added ListService with lists/statuses endpoint, to TwitterApiClient
* Updated the CompactTweetView photo display to support a range of aspect ratios to reduce cropping
* Made portrait oriented photos square cropped
* Added TwitterListTimeline to support Timelines of Tweets from Twitter Lists
* Fixed bug in SearchTimeline preventing some filter queries from being used
* Removed cobalt performance-metrics dependency
* Improved SearchTimeline results quality
* Reduced TweetView full name font weight on newer API versions for design consistency
* Removed tweet-composer dependency on twitter-core

1.5.0: May 7, 2015
------------------

* Added CollectionTimeline to show Tweets from a Twitter Collection
* Added refresh method to TimelineListAdapters
* Updated Picasso dependency version to 2.5.2, OkHttp required to be 2.0 or greater
* Removed final from TimelineCursor class

1.4.0: April 2, 2015
--------------------

* Added retweet display support to show the original Tweet with “retweeted by” attribution
* Added Identifiable interface to abstract models with getId() method. Made Tweet and User models implement Identifiable
* Timelines Support
* Added UserTimeline to show Tweets for a particular user
* Added SearchTimeline to show Tweets that match a search query
* Added TweetTimelineListAdapter for providing ListViews with a scrollable Timeline of Tweets

1.3.1: February 26, 2015
------------------------

* No changes

1.3.0: January 29, 2015
-----------------------


1.1.1: December 15, 2014
------------------------

* Included licensing information in pom
* Improved ProGuard configuration by bundling Consumer ProGuard Config file with AAR
* Fixed Twitter login crash that could occur on some devices that do not have the Twitter for Android app installed (see https://twittercommunity.com/t/authorization-crashes-on-some-android-devices-without-official-twitter-app/28456)
* Fixed allCaps Login with Twitter and Share Tweet buttons on API 21

1.1.0: November 21, 2014
------------------------

* Updated to Java 7 and Build Tools 21. Customers are required to use Build Tools version 19 or above.
* OAuth Echo support
* Renamed “Sign in” button to “Log in with Twitter”
* Improved layout preview of tweet views
* Bug fixes

TwitterCore
===========

3.2.0 Nov 13, 2017
-------------------

* Fixed issue that caused startup delays
* Moved build to Java 8

3.1.0 July 26, 2017
-------------------

* Bumped version to 3.1.0

3.0.0 May 25, 2017
------------------

* Removed dependency on Fabric
* Updated Retrofit and OkHttp dependencies
* Request email permission screen removed
* For more details checkout `Upgrade to Twitter Kit 3.0 <Upgrade-to-Twitter-Kit-3.0>`__

2.3.1 February 23, 2017
-----------------------

* Removed unused Vine utility method

2.3.0 December 8, 2016
----------------------

* Normalized user-agent to avoid errors with non-ascii characters
* Added symbols to Tweet entities

2.2.0 November 10, 2016
-----------------------

* Added support for user defined OkHttpClient

2.1.0 October 6, 2016
---------------------

* Fixed IllegalArgumentException in GuestAuthenticator

2.0.1 September 22, 2016
------------------------

* Updated ProGuard rules for OkHttp3 and Retrofit2
* Removed test-only pseudo locales from translations
* Moved TwitterCollection from internal package to models
* Minor bug fixes

2.0.0 August 11, 2016
---------------------

* Dropped support for API versions before API 14 (ICS)
* Migrated to Retrofit 2.0 and OkHttp 3.2
* TwitterApiClient now automatically refreshes expired guest tokens
* Removed previously deprecated methods and classes
* Removed all public reference to Application Authentication
* Fixed issue parsing withheldInCountries field in User object
* Added altText field to MediaEntity object
* Added Quote Tweet to Tweet object

1.7.0 June 23, 2016
-------------------

* Added support for Vine in Tweets
* Enabled extended Tweet display. Display full Tweet text when total length of a Tweet exceeds 140 characters

1.6.8 June 15, 2016
-------------------

* Fix Fake ID exploit

1.6.7 June 1, 2016
------------------

* Updated Fabric dependency

1.6.6 April 27, 2016
--------------------

* Fixed security issue where certificate pinning was not happening for some requests

1.6.5 March 4, 2016
-------------------

* Removed Verisign Class 3 Certificate from pinning list
* Fixed JavaDocs

1.6.4 February 4, 2016
----------------------

* Fixed issue retrieving auth token when using OkHttp 2.3+

1.6.3 January 12, 2016
----------------------

* Bug fixes

1.6.2 December 4, 2015
----------------------

* Added VideoInfo field to MediaEntity to support video and animated gif playback

1.6.1 November 12, 2015
-----------------------

* Updated translations

1.6.0: October 19, 2015
-----------------------

* Removed usage of deprecated Apache HTTP Client constants
* Moved GuestCallback from internal package
* Moved AspectRatioImageView from tweet-ui to twitter-core
* Moved tweet-ui UserUtils from tweet-ui to twitter-core

1.5.0: August 31, 2015
----------------------

* Raise Min SDK version from 8 to 9 to support SHA2 TLS certs
* Added MediaService to support media upload
* Added a StatusService update endpoint which accepts mediaIds

1.4.1: July 16, 2015
--------------------

* Fixed critical issue where Twitter sessions are lost when using ProGuard
* Added unretweet endpoint to StatusesService
* Switched CollectionService from the beta collections endpoint to the public collections endpoint

1.3.1: February 26, 2015
------------------------

* Bug fixes

1.3.0: January 29, 2015
-----------------------

* Added logInGuest method to TwitterCore to enable guest access to statuses endpoints user_timeline, show, and lookup, search/tweets, and favorites/list

TweetComposer
=============

3.2.0 Nov 13, 2017
-------------------

* Moved build to Java 8

3.1.0 July 26, 2017
-------------------

* Bumped version to 3.1.0

3.0.0 May 25, 2017
------------------

* App card creation removed from composer
* For more details checkout `Upgrade to Twitter Kit 3.0 <Upgrade-to-Twitter-Kit-3.0>`__

2.3.1 December 22, 2016
-----------------------

* Restricted Broadcast Intents to current application to avoid leaking sensitive information

2.3.0 December 8, 2016
----------------------

* Updated Twitter Core dependency

2.2.0 November 10, 2016
-----------------------

* Updated Twitter Core dependency

2.1.0 October 6, 2016
---------------------

* Updated Twitter Core dependency to version 2.1.0

2.0.1 September 22, 2016
------------------------

* Added translations
* Removed test-only pseudo locales from translations

2.0.0 August 11, 2016
---------------------

* Dropped support for API versions before API 14 (ICS)
* Updated Twitter Core Dependency

1.0.5 June 15, 2016
-------------------

* Updated Twitter Core Dependency

1.0.4 June 1, 2016
------------------

* Updated Fabric dependency
* Allow #hashtags to be pre-filled in Composer

1.0.1 October 29, 2015
----------------------

* Renamed drawables to avoid name collisions

1.0.0 October 19, 2015
----------------------

(Beta) Added Tweet Composer with support for App Cards. This requires a dependency on TwitterCore

0.9.0: August 31, 2015
----------------------

* Raise Min SDK version from 8 to 9 to support SHA2 TLS certs

0.7.3: February 26, 2015
------------------------

TweetUi
=======

3.2.0 Nov 13, 2017
-------------------

* Moved build to Java 8
* Narrowed support-v4 dependency to support-compat and support-core-ui

3.1.0 July 26, 2017
-------------------

* Added support for RecyclerView
* Linkified profile image and entities (hashtags, user mentions, and symbols)

3.0.0 May 25, 2017
------------------

* Fixed crash when liking a quote Tweet
* Fixed crash with multi-touch gestures in ViewPager
* For more details checkout `Upgrade to Twitter Kit 3.0 <Upgrade-to-Twitter-Kit-3.0>`__

2.3.2 February 23, 2017
-----------------------

* Videos less than 6.5 seconds now loop
* Fixed issue where quote Tweets with media showed both media and quote Tweet. Only media should be shown if both are included
* Added ability to set geocode for SearchTimeline builder

2.3.1 December 22, 2016
-----------------------

* Improved RTL mirroring for Tweet views
* Dates are now properly localized for non-English locales

2.3.0 December 8, 2016
----------------------

* Added support for timeline filtering

2.2.0 November 10, 2016
-----------------------

* Added support for quote Tweet display

2.1.0 October 6, 2016
---------------------

* Added multi-photo support for inline views and the fullscreen gallery view

2.0.1 September 22, 2016
------------------------

* Removed test-only pseudo locales from translations

2.0.0 August 11, 2016
---------------------

* Dropped support for API versions before API 14 (ICS)
* Updated Twitter Core dependency
* Removed previously deprecated methods and classes
* Added contentDescription for media based on altText field

1.11.1 July 1, 2016
-------------------

* Allow non-filtered search results for SearchTimeline

1.11.0 June 23, 2016
--------------------

* Added support for Vine in Tweets

1.10.3 June 15, 2016
--------------------

* Updated Twitter Core Dependency

1.10.2 June 1, 2016
-------------------

* Updated Fabric dependency

1.10.1 April 27, 2016
---------------------

* Added HLS playback support. Removed WebM playback support
* Added loading and buffering spinners to video player
* Only show play button and media badge if playback is supported

1.10.0 March 4, 2016
--------------------

* Added click listeners on Tweet views for URL and media clicks
* Fixed JavaDocs

1.9.1 February 4, 2016
----------------------

* Added GIF or duration badge to media view

1.9.0 January 12, 2016
----------------------

* Added basic photo viewer
* Added custom controls for video playback
* Enabled looping for animated gifs

1.8.0 December 4, 2015
----------------------

* Added support for video and animated GIF playback

1.7.0 November 12, 2015
-----------------------

* Changed TweetView favorite stars to like hearts

1.6.0: October 19, 2015
-----------------------

* Added Tweet Composer with support for App Cards

1.5.0: August 31, 2015
----------------------

* Raise Min SDK version from 8 to 9 to support SHA2 TLS certs

1.3.1: July 16, 2015
--------------------

* Changed TweetUtils loadTweet(s) to take a TwitterCore Callback. Deprecate those taking a LoadCallback
* Changed TweetViewFetchAdapter setTweetIds to take a TwitterCore Callback. Deprecate the method taking a LoadCallback

1.0.6: February 26, 2015
------------------------

* Fixed strict mode violations when using OkHttp on Fabric start

1.0.5: January 29, 2015
-----------------------

* Various bug fixes

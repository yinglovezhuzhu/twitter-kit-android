Twitter Kit provides easy integration with the `MoPub SDK <https://www.mopub.com/resources/docs/>`_, which makes it easy to monetize your app with native ads in `timelines <Show-Timelines>`__. With just a few lines of code, you can:

* Display engaging native ads inside `Twitter timelines <Show-Timelines>`__
* Implement native ad `best practices <https://www.mopub.com/resources/docs/ad-formats-best-practices/>`_
* Style ads with customizable light and dark styles to match the exact look and feel of your app

Create a MoPub Account and Ad Unit
==================================

Note, it is recommended that you create a new native ad unit ID for timeline ads.

Follow these 3 steps to set up an ad unit ID:

* `Create a MoPub account <https://app.mopub.com/account/register/>`_ if you do not have one already
* `Add your application to the MoPub dashboard <https://www.mopub.com/resources/docs/mopub-ui-account-setup/creating-apps-ad-units/>`_ once you have a MoPub account
* `Create a new native ad unit <https://www.mopub.com/resources/docs/mopub-ui-account-setup/creating-apps-ad-units/>`_ with the desired configurations

To learn more about MoPub's ad exchange and marketplace, `see more <https://www.mopub.com/resources/docs/>`_

Add Twitter-MoPub Dependency
============================

Add :code:`twitter-mopub` to your build.gradle dependencies:

.. code-block:: groovy

  dependencies {
    ...

    compile('com.twitter.sdk.android:twitter-mopub:3.1.1@aar') {
        transitive = true;
    }
  }

Twitter-MoPub Adapter
=====================

:code:`twitter-mopub` provides :code:`TwitterMoPubAdAdapter`, which will contact MoPub's ad server to determine ad position, register an ad renderer :code:`TwitterStaticNativeAdRenderer`, and load ads in a `Timeline <Show-Timelines>`__.

Here is an example of using :code:`TwitterMoPubAdAdapter`:

.. code-block:: java

    public class UserTimelineFragment extends ListFragment {

        private MoPubAdAdapter moPubAdAdapter;

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            ...

            final UserTimeline userTimeline = new UserTimeline.Builder().screenName("twitterdev").build();
            final TweetTimelineListAdapter adapter = new TweetTimelineListAdapter.Builder(getActivity())
                    .setTimeline(userTimeline)
                    .setViewStyle(R.style.tw__TweetLightWithActionsStyle)
                    .setOnActionCallback(actionCallback)
                    .build();

            moPubAdAdapter = new TwitterMoPubAdAdapter(getActivity(), adapter);
            final TwitterStaticNativeAdRenderer adRenderer = new TwitterStaticNativeAdRenderer();
            moPubAdAdapter.registerAdRenderer(adRenderer);
            moPubAdAdapter.loadAds(BuildConfig.MOPUB_AD_UNIT_ID);

            setListAdapter(moPubAdAdapter);
        }

        @Override
        public void onDestroy() {
            // You must call this or the ad adapter may cause a memory leak
            moPubAdAdapter.destroy();
            super.onDestroy();
        }
    }

Styling
=======

:code:`twitter-mopub` provides predefined styles, :code:`tw__ad_LightStyle` and :code:`tw__ad_DarkStyle`. Styles are defined by the following attributes:

.. code-block:: xml

    <!--attrs.xml-->
    <resources>
        <declare-styleable name="tw__native_ad">
            <attr name="tw__ad_container_bg_color" format="color"/>
            <attr name="tw__ad_card_bg_color" format="color"/>
            <attr name="tw__ad_text_primary_color" format="color"/>
            <!-- call to action -->
            <attr name="tw__ad_cta_button_color" format="color"/>
        </declare-styleable>
    </resources>

.. image:: http://twitter.github.io/twitter-kit-android/images/twitter-mopub-light-style.png
  :width: 320px


.. image:: http://twitter.github.io/twitter-kit-android/images/twitter-mopub-dark-style.png
  :width: 320px

The :code:`tw__ad_LightStyle` is configured by default in :code:`TwitterStaticNativeAdRenderer`. The desired style can be specified as follows:

.. code-block:: java

    final TwitterStaticNativeAdRenderer adRenderer = new TwitterStaticNativeAdRenderer(R.style.tw__ad_DarkStyle);

Custom Styling
--------------
Customizing the styling to match the look and feel of your app is easy:

.. code-block:: xml

    <!--styles.xml-->
    <style name="tw__ad_MyAppStyle" parent="tw__ad_LightStyle">
        <item name="tw__ad_cta_button_color">@color/custom_color_1</item>
        <item name="tw__ad_text_primary_color">@color/custom_color_2</item>
        <item name="tw__ad_container_bg_color">@color/custom_color_3</item>
        <item name="tw__ad_card_bg_color">@color/custom_color_4</item>
    </style>

If an attribute is not set, the value defaults to values from :code:`tw__ad_LightStyle`. The custom style reference needs to be passed to :code:`TwitterStaticNativeAdRenderer`.

.. code-block:: java

    final TwitterStaticNativeAdRenderer adRenderer = new TwitterStaticNativeAdRenderer(R.style.tw__ad_MyAppStyle);

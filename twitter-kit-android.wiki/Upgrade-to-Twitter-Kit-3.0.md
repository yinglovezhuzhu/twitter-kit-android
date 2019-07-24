Twitter Kit no longer has a dependency on Fabric. In order to continue using functionality provided by Fabric you should directly include Fabric as a dependency.

Twitter Kit 3.0 includes a number changes that must be taken into consideration when upgrading from 2.x. The sections below outlines these changes.

:strong:`Binary Distribution`

Twitter Kit binaries are no longer distributed through Fabric's maven repository. All binaries are now distributed through `Bintray jcenter <https://bintray.com/bintray/jcenter>`_. Most Android projects should already include jcenter so no changes are required.

:strong:`Twitter Kit Initialization`

Calling :code:`Fabric.with(Fabric)` is no longer required. Twitter Kit is now initialized by calling :code:`Twitter.initialize(TwitterConfig)`

Individual kits are now initialized the first time :code:`getInstance()` method is called on each component. If :code:`getInstance()` is called before calling :code:`Twitter#initialize()` an :code:`IllegalStateException` will be thrown. For more details see `Twitter Installation <Getting-Started>`__

:strong:`Request email permission screen removed`

The permission screen shown when calling :code:`TwitterAuthClient#requestEmail()` has been removed. We no longer require 3rd party apps to display the permission screen to request the user’s email. The :code:`requestEmail()` method will now return the user’s email address without displaying the permission screen.

:strong:`App card creation removed from composer`

Twitter Kit no longer supports programmatic creation of App Cards through the native composer. The native composer will remain and will be expanded to include additional features in the coming months. For more details see `Compose Tweets <Compose-Tweets>`__

- [Removed] The :code:`Card` class and :code:`ComposerActivity.Builder#card` method was removed.
- [Added] The :code:`ComposerActivity.Builder#image` method was added for attaching an image to the Tweet.
- [Added] The :code:`ComposerActivity.Builder#text` method was added for setting default composer text.

:strong:`Miscellaneous`

- [Changed] :code:`Tweet#extendedEtities` was changed to :code:`Tweet#extendedEntities`
- [Removed] :code:`TwitterCore#logIn(Activity, Callback)` was removed use :code:`TwitterLoginButton` or :code:`TwitterAuthClient` instead.
- [Removed] :code:`TwitterCore#logOut()` was removed use :code:`TwitterCore.getInstance().getSessionManager().clearActiveSession()` instead.

Some classes in :code:`io.fabric.sdk.android.*` have been moved to :code:`com.twitter.sdk.android.core.*`. These include:

- :code:`Logger`
- :code:`DefaultLogger`
- :code:`ExecutorUtils`
- :code:`CommonUtils`
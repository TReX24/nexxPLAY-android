# nexxPLAY Android (v.3.x.x)
We would like to enable you a smooth integration of NexxPlay into your existing project. The following guide will explain everything you need to know. There is a demo project "demo", where you can find a running example of the player.

`Please read the installation guide again if you used v.1 or v.2 of nexxPlay for Android.`

## Installation guide

This section shows how to integrate the media player into an Android app.

### Gradle

Let us start by adding the nexx.tv Maven repository in your root Gradle file. Under the section allprojects you can find the repositories section. Please add the following line:

```
...
maven { url "https://www.myget.org/F/nexxtv/maven" }
...
```

We are almost done for the Gradle files. The last step is to include the following line to your app Gradle file (see app/build.gradle). 

```
dependencies { 
    ...
    implementation 'tv.nexx:nexxplay-android:3.+'
    ...
}
```
That way the build script will look for the newest version of nexxPlay everytime a build of your app is being initiated. However if you want to stick to a specific version, you can always find a list of the releases
[on the nexxPlay release site](https://www.myget.org/feed/nexxtv/package/maven/tv.nexx/nexxplay-android)


### User Interface
NexxPlay needs a root anchor view which should be a FrameLayout. Please add something likes this to your layout, depending on your needs:

```
    <FrameLayout
        android:id="@+id/root"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

### Activity

Create the NexxPlayer object providing API by calling the factory method within your
Activity: 

```
this.nexxPlayerAndroid = NexxFactory.createNexxPlayer(this);
```

The NexxPlayer needs a single view. The root view is the view group where the media player control layout (the player
skin) is shown. Also the player needs the Acitivity's window
```
ViewGroup root = (ViewGroup) findViewById(R.id.root);
this.nexxPlayerAndroid.setViewRoot(root);
this.nexxPlayerAndroid.setWindow(getWindow());
```

### Player methods

The NexxPlay Android SDK implements two interfaces: NexxPlayer which is the interface for all media functions and NexxPlayerAndroid which is the Android specific interface.

#### NexxPlayerAndroid
The SDK needs to be triggered by some of the Android lifecycle methods. There are the following methods in NexxPlayerAndroid that need to be called in the Activit's particular method:
```
void onActivityResume();
void onActivityPause();
void onActivityDestroyed();
```

#### NexxPlayer
Let us get to the exciting part! With the NexxPlayer interface, you can control all the media related functionality. Here is a list of all methods:

##### void startPlay(String cid, String client, PlayMode playMode, String param, int startPosition, int startItem);
Start media

##### void swapToPos(int newPosition);
Directly swap to position in seconds in current media

##### void swap(String param);
Use current player instace to load another media

##### MediaData getMediaData();
Obtain an object containing meta-data of the current media item.

##### EnhancedMediaData getEnhancedMediaData();
Obtain an object containing meta-data of the current media item.

##### void setSSL(boolean ssl);
Set if ssl should be used or not

##### void enableCordova();
Enable Cordova mode.

##### void setDataMode(DataMode dataMode);
Set static or usual method to receive data. by default, API is set.

##### void clearCaches();
clear all caches

##### void setPlayLicense(int playLicense);
Adjust domain settings

##### void setWebURLRepresentation(String url);
set web url for given media to add in vast url

##### void setLoaderSkin(LoaderSkin skin);
skin of loading indicator

##### void setStreamingFilter(String streamingFilter);
Streaming filter for bit-rates

##### void setViewParentID(String viewParentID);
e.g. playlist-PLAYLIST-ID

##### void setUserHash(String hash);
set hash for current user

##### float getCurrentTime();
get the current played time

#### Override methods

##### void overrideMenu(MenuVisibilityMode mode);
Specifies how the menu should be shown

##### void overrideAutoPlay(AutoPlayMode mode);
Specifies how the auto-play should behave

##### void overrideStartMuted(MutedMode mode);
Specifies whether or not to start muted.

##### void overrideExitPlayMode(ExitPlayMode mode);
Specifies what happen at exit.

##### void overrideAutoPlayNext(AutoPlayNextMode mode);
Specifies how auto-next should behave.

##### void overrideCaptionMode(String captionMode);
Set the caption mode ('none', 'select', 'selectandstart' or 'always')

##### void overridePreroll(String url);
Manually set the VAST PreRoll URL

##### void overrideMidroll(String url, int interval);
Manually set the VAST MidRoll URL AND the Interval in Minutes.

##### void overridePostroll(String url);
Manually set the VAST PostRoll URL

##### void overrideAdProvider(String providerCode);
Manually set the VAST Provider Code (as defined within nexxOMNIA)

##### void overrideAdType(String adType);
Define ad consuming type. Possible values are ima / vast

##### void overrideTitles(int allowTitles);
Define if titels should be showen. Possible values are 0 = don't show, 1 = always show, 2 = show in fullscreen only

### Events
Your application may subscribe for receiving notifications from the player calling:

```
NexxPlayerNotification.Listener listener = this.nexxPlayer.addEventListener(listener);
```
There are multiple events, which may or may not have additional data as a secondary paramenter:

    ERROR,
    META_DATA_RECEIVED,
    READY,
    SHOW_UI,
    HIDE_UI,
    ENTER_FULLSCREEN,
    EXIT_FULLSCREEN,
    START_PLAYBACK,
    START_PLAY,
    PLAY,
    PAUSE,
    ENDED,
    PLAY_POS,
    AD_STARTED,
    AD_ENDED,
    AD_ERROR,
    AD_CLICKED,
    AD_RESUMED,
    AD_CALLED,
    MUTE,
    UNMUTE,
    PLAYED_25_PERCENT,
    PLAYED_50_PERCENT,
    PLAYED_75_PERCENT,
    PLAYED_95_PERCENT

### Android TV
There is an additional AAR file for Android TV which uses the newest implementation of ExoPlayer as well as the Leanback library for controls.  Please add this AAR to your libs folder and add the following lines to your gradlew file:
```
implementation 'com.google.android.exoplayer:exoplayer:2.8.1'
implementation 'com.google.android.exoplayer:extension-mediasession:2.8.1'
implementation 'com.google.android.exoplayer:extension-leanback:2.8.1'
implementation 'com.android.support:support-v13:27.0.2'
```




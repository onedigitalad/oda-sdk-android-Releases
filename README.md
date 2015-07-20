# ODA SDK For Android


This document describes how to get started using the ODA SDK For Android which gives you valuable insights into your app's visitors, your marketing campaigns and much more, so you can optimize your strategy and experience of your visitors.

### Developer Documentation

You can get started with using OneDigitalAd Analytics on Android in minutes. Just follow the steps below:

ODA SDK should work fine with Android API Version >= 9 (Android 2.2+ Gingerbread and up). This is required since google advertiser id was introduced from this version
Automatic Events logging is available on API level >= 14.

Check out the two example projects as below

1. [SunshineProject Integrated ODA SDK Source](https://github.com/onedigitalad/Sunshine-Version-2)
    [Diff with original](https://github.com/udacity/Sunshine-Version-2/compare/sunshine_master...onedigitalad:sunshine_master)
2. [Google example Universal Music Player Integrated ODA SDK Source](https://github.com/googlesamples/android-UniversalMusicPlayer)
    [Diff with original](https://github.com/googlesamples/android-UniversalMusicPlayer/compare/master...onedigitalad:master)

|#  |Step                                                                       |
|---|---                                                                        |
|1  |  Installation: [Android Studio](#android-studio), [Eclipse](#eclipse)     |
|2  | [Initialize](#2-initialization) SDK                                       |
|3  | [Setting User Attributes](#3-user-attributes) (optional)                  |
|4  | [Tracking Events](#4-track-events) (optional)                             |



## 1. Installation

### Android Studio

1. _In your module’s build.gradle, add the url to the sdk._

    ```
    repositories {
        maven { url "https://github.com/onedigitalad/oda-sdk-Android-Releases/raw/master/AndroidStudio/" }
    }
    ```

2. _In your *module’s* build.gradle dependencies (not your project's build.gradle), compile OneDigitalAd Analytics and its dependencies._

    ```
    dependencies {
        //OneDigitalAd Analytics
        compile("com.oda.sdk:oda-tracking-sdk:+@aar")

        //Dependencies for OneDigitalAd Analytics if reading AdvertiserId
        compile("com.google.android.gms:play-services-gcm:7.5.0")
    }
    ```

3. _Override your Application’s onCreate() method (not your main activity) and call ODATracker.start(). If you don't have an Application class, create one. It should look like this:_

    ```java
    public class ExampleApplication extends Application {
        @Override
        public void onCreate() {
            super.onCreate();
            ODATracker.start(this, "YOUR OneDigitalAd Analytics API KEY");
        }
    }
    ```
   Don't forget to add application name to your `AndroidManifest.xml` file.

   ```xml
       <application android:name=".ExampleApplication">
           <!-- activities goes here -->
       </application>
   ```


4. _Now, add the proper permissions, and the Application class to your app’s AndroidManifest.xml in the Application tag._

     ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <application
        android:name=".ExampleApplication"
    ...
    ```

5. _That's it! Now build and run your app with OneDigitalAd Analytics!_

---

### Eclipse

1. _Download the OneDigitalAd Analytics.jar [here](https://github.com/OneDigitalAd Analytics/OneDigitalAd Analytics-Android-SDK/raw/master/OneDigitalAd Analytics.jar)_
2. _Copy the jar into your 'libs' directory in your project._
3. _Right click the jar in Eclipse, click Build Path > add to build path_
4. **NEW:** _Add Google Play Services to your project by following the steps listed [here.](http://developer.android.com/google/play-services/setup.html) Be sure to change the dropdown to "Eclipse with ADT"_
5. _Add the following to your Proguard rules:_

    (Only if using Proguard)

    ```
    -keep class android.support.v4.app.Fragment { *; }
    -keep class android.support.v4.view.ViewPager
    -keepclassmembers class android.support.v4.view.ViewPager$LayoutParams {*;}
    ```
6. _Override your Application’s onCreate() method (not your main activity) and call ODATracker.start(). If you don't have an Application class, create one. It should look like this:_

    ```java
    public class ExampleApplication extends Application {
        @Override
        public void onCreate() {
            super.onCreate();
            ODATracker.start(this, "YOUR OneDigitalAd Analytics API KEY");
        }
    }
    ```
   Don't forget to add application name to your `AndroidManifest.xml` file.

   ```xml

       <application android:name=".ExampleApplication">
           <!-- activities goes here -->
       </application>
   ```

7. _Now, add the proper permissions, and the Application class to your app’s AndroidManifest.xml in the Application tag._

     ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <application
        android:name=".ExampleApplication"
    ...
    ```

8. _That's it! Now build and run your app, you can start creating experiments with OneDigitalAd Analytics!_

---

## Setup

### 2. Initialization

OneDigitalAd Analytics can be started with a few options to help you use it during development.

First, the base method:

```java
ODATracker.start(this, "Your Api Key");
```


---

### 3. User Attributes (Optional)

Its possible to send custom user attributes to OneDigitalAd Analytics using a JSONObject of user info.

The main possible fields are:

|Parameter  |Type         |
|---      |---          |
|email      | String    |
|user_id    | String    |
|firstname  | String    |
|lastname   | String    |
|name     | String    |
|age      | Number    |
|gender     | String    |

You can also add anything else you would like to this JSONObject and it will also be passed to OneDigitalAd Analytics.

An example with custom data:

```java
JSONObject attributes = new JSONObject();
attributes.put("email", "John.Smith@example.com");
attributes.put("name", "John Smith");
attributes.put("age", 25);
attributes.put("gender", "male");
attributes.put("avatarUrl", "https://example.com/John_Smith.png");
attributes.put("someCustomAttribute",50);
attributes.put("paidSubscriber", true);
attributes.put("subscriptionPlan", "yearly");

ODATracker.setUserAttributes(attributes);
```

---

### 4. Track Events

####Automatic Events

Some events are automatically tracked by OneDigitalAd Analytics. These events are:

* App Start
* Activity and/or Fragment load
* Activity and/or Fragment destroy
* Activity pause
* App background
* Viewpager changes
* App Crash

App terminate is also tracked, but this is only true when your MAIN activity is at the bottom of your activity stack, and the user exits the app from that activity.

No changes are needed in your code for this event tracking to occur.

####Custom Events

To log your own events, simply call:

```java
ODATracker.logEvent("Your Event Name");
```

You can also log events with numerical values:

```java
Number num = 0;
ODATracker.logEvent("Your Event Name", num);
```

And with custom object data:

```java
Number num = 0;
JSONObject customInfo = new JSONObject();
customInfo.put("some title",someValue)
ODATracker.logEvent("Your Event Name", num, customInfo);
```

####Revenue Logging

Its also possible to log revenue.

Revenue logging is the exact same as event logging, only call `logRevenue`:

```java
Number someRevenue = 10000000;
ODATracker.logRevenue("Revenue Name", someRevenue);
```

And similarly, with custom object data:

```java
Number someRevenue = 10000000;
JSONObject customInfo = new JSONObject();
customInfo.put("some rag",someValue)

ODATracker.logRevenue("Revenue Name", someRevenue, customInfo);
```


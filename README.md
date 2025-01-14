# Fork info

This fork of the Twilio Client Phonegap plugin bundles the headers and libraries so that it is self contained. Primary goal was to enable use via Adobe Build.

Bundled version of Twilio client SDK: 1.2.11-rc5

# Twilio Client Phonegap plugins for iOS and Android (version 1.0.7)

These are Phonegap plugins that expose the same JS API as Twilio Client for web as much as possible, meaning you should be able to use the same Twilio Client code from your web application inside of your Phonegap application with few if any modifications. 

# Latest versions tested with this plugin
#### (as of August 16, 2016)
- Cordova 6.3.1
- Cordova Android 5.2.1
- Cordova iOS 4.1.0
- Twilio Client for iOS 1.2.8
- Twilio Client for Android 1.2.17
- XCode 7.3
- Android SDK 23

# Android Gradle Support
- Version 1.0.6 and above now downloads the latest version of the Twilio SDK from jcenter - you will need to use the Cordova Android plugin 5.2.1 for this, as the version of Gradle needed is 2.1.3, and 5.2.1 also includes jcenter as a default repository. 

# Android Support Library
- Versions 1.0.4 and earlier of the plugin required you to include the Android support library v4 in the lib. Cordova now has a solution for requiring Android libs through Gradle dependencies. This will save you a step on installation, as well as making this plugin (as of versin 1.0.5) compatible with other plugins such as the PhoneGap Push plugin. One handy tip I found on that plugin is that you may need to update your Android support library using this command line tool, if you get an error saying that the Android Support v4 library can not be found:

`android update sdk --no-ui --filter "extra"`

Please file an issue if you have problems with the Android support library or any compatibility issues with other plugins.

# Example application
https://github.com/jefflinwood/TwilioClientPhoneGapExampleApp

# PhoneGap/Cordova Overview

- Install the most recent version of Cordova (as of this writing, 6.3.1 tools  - http://http://cordova.apache.org/ 
- Install plugman - https://github.com/apache/cordova-plugman

# Both Platforms at once

##Instructions
```
cordova plugin add  https://github.com/jefflinwood/twilio_client_phonegap.git
```

# iOS only

##Instructions

```
 plugman install --platform ios --project platforms/ios --plugin https://github.com/jefflinwood/twilio_client_phonegap.git

```

(These steps can be skipped)
- After installing the Twilio Client plugin, you will need to download and install the Twilio Client SDK for iOS - follow the directions provided after plugman finishes.

# Android only

## Instructions

```
plugman install --platform android --project platforms/android --plugin https://github.com/jefflinwood/twilio_client_phonegap.git

```

## Additional Features

In addition to the standard features of the Twilio Client JS Library, you can also use the included showNotification and cancelNotification functions to display a UILocalNotifcation to the user when during an incoming call while the app is running in the background:

```javascript
Twilio.Connection.showNotification("Notification Text", "notification_sound.wav");
```

```javascript
Twilio.Connection.cancelNotification();
```

You can also turn the device's speaker phone on or off during a call using the following method:

```javascript
Twilio.Connection.setSpeaker("on");
```

## Changelog
- 1.0.7 - Added Marshmallow/SDK 23 support for runtime permissions
- 1.0.6 - Updated Android platform for plugin to pull in Twilio Android SDK using Gradle

## Limitations

This is plugin is a first cut and should be considered alpha. Please use it and break it :) Report any issues using the Github issue tracker.

Some of the event handlers are currently no-ops because of differences between the web SDK and the iOS SDK, i.e. they both expose events the other does not, e.g. Twilio.Device.cancel is a no-op and there is no JS SDK notion of `-(void)deviceDidStartListeningForIncomingConnections:(TCDevice*)device`, etc. 

Twilio.Connection objects are just proxy objects that delegate to the Objective-C layer.

Sounds are currently disabled and the JS methods that control them are no-ops because enabling them causes `EXC_BAD_ACCESS` errors. Will fix.

Methods that interrogate the client device or connection and return a result e.g. `Twilio.Device.status()` take a callback function with the response as the argument. Unfortunately I think this is unavoidable due to communication between the Phonegap JS and iOS layers being strictly asynchronous, e.g.

```javascript
Twilio.Device.connect(function(connection) {
    alert(connection.status());
})
```

becomes:

```javascript
Twilio.Device.connect(function(connection) {
    connection.status(function(status) { alert(status) });
})
```

Twilio Client documentation: http://www.twilio.com/docs/client/twilio-js


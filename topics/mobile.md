# Mobile development

## Native

No compilation needed, as the app is written in the native language.

-   Java (Android) 77% share
-   Swift (iPhone) 19% share

## Hybrid

Like Electron, but for mobile devices.

-   PhoneGap - Same as Cordova, vanilla JS, paid version.
-   Cordova - Same as PhoneGap, vanilla JS, open source.
-   Ionic - Angular UI library on top of Cordova that gives a native look.

## Compiled

**Only** the UI components are compiled to their native equivalents. The rest runs in the language runtime.

-   React Native (Javascript) Have to build your own components.
-   Native Script (Javascript) Less popular
-   Flutter (Dart)

# Android

## Setup Overview

-   JDK
-   SDK
    -   Packages
-   Environment Variables
-   NodeJS
-   Cordova

## API Level

The Android platform provides a framework API that applications can use to interact with the underlying Android system. Updates to the framework API are designed so that the new API remains compatible with earlier versions of the API.

| Codename    | Version | API | Distribution |
| ----------- | ------- | --- | ------------ |
| Oreo        | 8.0.0   | 26  | 11%          |
| Nougat      | 7.0.0   | 24  | 20%          |
| Marshmallow | 6.0.0   | 23  | 22%          |

### Java Development Kit (JDK)

The JDK (Java Development Kit) contains the JRE (Java Runtime Environment).

Open JDK = open-source variant of the JRE and JDK.

Java version 1.8.0_5 = JDK 8 update 5

```bash
sudo apt update
sudo apt install openjdk-8-jdk

java -version # java version "1.8.0_181"

# Add this line to ~/.profile
export JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk-amd64"

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin

# Reload configuration
source ~/.profile
```

## Gradle

As of Cordova-Android 6.4.0, Gradle is now required to be installed to build Android. Gradle requires Java Development Kit (JDK) 7 or higher in order to work.

```bash
wget https://services.gradle.org/distributions/gradle-4.9-bin.zip
sudo unzip -d /opt/gradle gradle-4.9-bin.zip
```

## Android SDK

By default, the Android SDK does not include everything you need to start developing.

The Android SDK can be broken down into several components. These include:

-   SDK-tools
-   Platform-tools
-   Build-tools
-   The Android Debug Bridge (ADB)
-   Android Emulator

Arguably the most important parts of this package are in the SDK-tools. You will need these tools regardless of which version of Android you are targeting.

These are what will actually compile your code along with any data and resource files into an APK, an Android package, which is an archive file with an `.apk` suffix.

One APK file contains all the contents of an Android app and is the file that Android-powered devices use to install the app.

**From the SDK you really only need sdk-tools, platform-tools, build-tools and the latest API.**

```bash
sudo apt-get install android-sdk
```

```bash
wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
sudo unzip -d /home sdk-tools-linux-4333796.zip

# Add this line to ~/.profile
export PATH=/opt/pradip/tools:/opt/pradip/tools/bin:$PATH

# Reload configuration
source ~/.profile
```

After installing the Android SDK, you must also install the packages for whatever API level you wish to target. It is recommended that you install the highest SDK version that your version of cordova-android supports.

```bash
sdkmanager --list
```

Android Platform SDK for your targeted version of Android
Android SDK build-tools version 19.1.0 or higher
Android Support Repository (found under "Extras")

## Environment Variables

We need to configure and export the environment variables so that the executables can be run directly from anywhere. Add them to `~/.bashrc` to make them permanent.

```bash
sudo vim ~/.profile
```

add following code to the end of the file...

```bash
# Gradle
PATH=$PATH:/opt/gradle/gradle-4.9/bin

# Android SDK
ANDROID_HOME="/usr/lib/android-sdk/"
PATH="${PATH}:${ANDROID_HOME}tools/:${ANDROID_HOME}platform-tools/"
```

save, exit and run source to relad the configuration.

```bash
source ~/.profile
```

## SDK Manager

https://www.androidauthority.com/android-sdk-tutorial-beginners-634376/  
https://www.androidauthority.com/how-to-install-android-sdk-software-development-kit-21137/

## Android Virtual Device Manager (AVD)

Create a virtual device where you set the desired device with the system image downloaded via the SDK manager, as well as the target API.

## Build Process

https://developer.android.com/studio/build

# Apache Cordova

Mobile apps can be created with HTML, CSS and Javascript.

The mobile app is a website running in a webview, like Electron for desktop. A webview is a wrapper i.e. a shell browser, without the menus, tabs etc.

The native device features can be accessed through the wrapper, not directly. Also, The UI components automatically adjust to the platform.

## History

-   PhoneGap was previously a product of Adobe.
-   To keep it open-source always and follow standards, PhoneGap codebase was handed over to Apache.
-   At Apache, it got a name change as Cordova.
-   And now it’s better known as Apache Cordova.
-   Ionic sits on top of Cordova as a UI library with Angular, to give the web app a native feel.

PhoneGap is paid.
Cordova is open source.
The codebase is the same.

If you use phonegap/Cordova you are using plain JavaScript (and can therefore choose to add any framework, including Angular, to that) whereas if you're using ionic then you're an AngularJs developer.

## Install

To run Cordova, node is required. NPM is used for intalling Cordova, along with the plarforms and plugins.

```bash
sudo npm i cordova -g
```

When the app is compiled, the target platform SDK is required.

-   Adroid Stuido for Android SDK.
-   Xcode for iPhone SDK.

## Create app

```bash
#cordova folder_name package_name app_name
cordova create myApp com.domain.app AppName
```

This will create the following:

-   `config.xml` - Old configuration (Don't use)
-   `hooks` - Special build tools go here.
-   `package.json` - Configuration
-   `platforms` - SKDs are stored here.
-   `plugins` - Like node_modules
-   `res` - Additional resources, like icons.
-   `www` - The web app lives here.
    -   css
    -   img
    -   js
    -   index.html

The phone doesn't call the apps by name. It uses a special internal name to reference them.

Reverse domain name notation is used as the naming convention for the packages. They are based on registered domain names, and are only reversed for sorting purposes.

For example, if a company making a product called `MyProduct` has the registered domain name `example.com`, they could use the reverse-DNS string `com.example.MyProduct` to describe it.

Reverse-DNS names are a simple way of reducing name-space collisions, since any domain name is registered by only one party at a time.

## Add platform

```bash
cordova platform add android
```

## Emulate app

This will load the app inside the emulator. You first need to create it with a system image for the target platform with AVD.

```bash
cordova emulate android
```

## Build app

This will build the app i.e. `.apk` file, which can be installed on an android device.

```bash
cordova build android
```

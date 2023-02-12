# Setup Overview

-   Java Development Kit (JDK)
-   Software Development Kit (SDK)
-   Latest API
-   Environment Variables

# API Level

The Android platform provides a framework API that applications can use to interact with the underlying Android system. Updates to the framework API are designed so that the new API remains compatible with earlier versions of the API.

| Platform    | Version | API | Share (2021) |
| ----------- | ------- | --- | ------------ |
| Jelly Bean  | 4.1     | 16  | 0.2%         |
| Jelly Bean  | 4.2     | 17  | 0.3%         |
| Jelly Bean  | 4.3     | 18  | 0.1%         |
| KitKat      | 4.4     | 19  | 1.4%         |
| Lollipop    | 5.0     | 21  | 0.7%         |
| Lollipop    | 5.1     | 22  | 3.2%         |
| Marshmallow | 6.0     | 23  | 5.1%         |
| Nougat      | 7.0     | 24  | 3.4%         |
| Nougat      | 7.1     | 25  | 2.9%         |
| Oreo        | 8.0     | 26  | 4.0%         |
| Oreo        | 8.1     | 27  | 9.7%         |
| Pie         | 9.0     | 28  | 18.2%        |
| Q           | 10.0    | 29  | 26.5%        |
| R           | 11.0    | 30  | 24.3%        |

# Java Development Kit (JDK)

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

As of Android 6.4.0, Gradle is now required to be installed to build Android. Gradle requires Java Development Kit (JDK) 7 or higher in order to work.

```bash
wget https://services.gradle.org/distributions/gradle-4.9-bin.zip
sudo unzip -d /opt/gradle gradle-4.9-bin.zip
```

# Software Development Kit (SDK)

By default, the Android SDK does not include everything you need to start developing.

The Android SDK can be broken down into several components. These include:

-   Core
    -   SDK-tools
    -   Platform-tools
    -   Build-tools
-   Extra
    -   The Android Debug Bridge (ADB)
    -   Android Emulator

Arguably the most important parts of this package are in the SDK-tools. You will need these tools regardless of which version of Android you are targeting.

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

# Latest API

After installing the Android SDK, you must also install the packages for whatever API level you wish to target. It is recommended that you install the highest SDK version that your version of Android supports.

```bash
sdkmanager --list
```

-   Android Platform SDK for your targeted version of Android
-   Android SDK build-tools version 19.1.0 or higher
-   Android Support Repository (found under "Extras")

# Andorind Package APK (.apk)

It contains all your code, along with any data and resource files.

It's the file that Android-powered devices use to install the app.

# Environment Variables

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

# SDK Manager

https://www.androidauthority.com/android-sdk-tutorial-beginners-634376/  
https://www.androidauthority.com/how-to-install-android-sdk-software-development-kit-21137/

# Android Virtual Device Manager (AVD)

Create a virtual device where you set the desired device with the system image downloaded via the SDK manager, as well as the target API.

# Build Process

https://developer.android.com/studio/build

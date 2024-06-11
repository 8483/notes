# Setup Overview

-   Java Development Kit (JDK)
-   Software Development Kit (SDK)
-   Latest API
-   Environment Variables

# Java Development Kit (JDK)

The **Java Development Kit** (JDK) contains the **Java Runtime Environment** (JRE).

**Open JDK** is the open-source variant of the **JRE** and **JDK**.

```bash
sudo apt update
sudo apt install openjdk-8-jdk

# Java version 1.8.0_5 = JDK 8 update 5

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

**API Level distribution**

The Android platform provides a framework API that applications can use to interact with the underlying Android system. Updates to the framework API are designed so that the new API remains compatible with earlier versions of the API.

| Version    | Codename         | SDK / API level | Release | Cummulative share |
| ---------- | ---------------- | --------------- | ------- | ----------------- |
| Android 14 | Upside Down Cake | 34              | 2023    | 16.3%             |
| Android 13 | Tiramisu         | 33              | 2022    | 42.5%             |
| Android 12 | Snow Cone        | 32 / 31         | 2021    | 59.5%             |
| Android 11 | Red Velver Cake  | 30              | 2020    | 75.7%             |
| Android 10 | Quince Tart      | 29              | 2019    | 84.5%             |
| Android 9  | Pie              | 28              | 2018    | 90.2%             |

# Android Package APK (.apk)

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

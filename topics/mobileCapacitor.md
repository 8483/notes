Capacitor allows Javascript web apps to act like native Android/iOS apps.

It wraps your web code into a native WebView app to "simulate" a mobile app, while offering native phone functionalities like camera and push notifications.

# WSL

Capacitor **DOES NOT** support running from a different "computer" from where you have the Android SDK installed.

WSL is like a "linux virtual machine" with limited access to your windows computer, and your project is there, so Capacitor can't run your project on windows as it lives in the "linux VM" (WSL).

There is a third party project that leverages adb to move and launch the app in windows
https://github.com/leo-petrucci/run-cap-on-android

# Android Studio and SDK

To develop Android applications using Capacitor, you will need:

-   Android Studio.
-   An Android SDK for the platform version you want to target. (`Android Studio > Settings > Frameworks > Android SDK`)

| Version    | Codename         | SDK / API level | Release | Cummulative share |
| ---------- | ---------------- | --------------- | ------- | ----------------- |
| Android 14 | Upside Down Cake | 34              | 2023    | 16.3%             |
| Android 13 | Tiramisu         | 33              | 2022    | 42.5%             |
| Android 12 | Snow Cone        | 32 / 31         | 2021    | 59.5%             |
| Android 11 | Red Velver Cake  | 30              | 2020    | 75.7%             |
| Android 10 | Quince Tart      | 29              | 2019    | 84.5%             |
| Android 9  | Pie              | 28              | 2018    | 90.2%             |

You do not need to separately install the Java Development Kit (JDK). Android Studio will automatically install the proper JDK for you.

# Setup

1. Install capacitor in the same directory.

-   As a new project.

```
npm init @capacitor/app
```

-   In an existing app. Requirements:

    -   A `package.json` file.
    -   A separate directory for built web assets such as `dist` or `www`.
    -   An `index.html` file at the root of your web assets directory.

```bash
npm i @capacitor/core
npm i -D @capacitor/cli
```

2. Initialize the app.

```
npx cap init
```

3. Install platforms i.e. added as dependencies to `package.json`.

```
npm i @capacitor/android @capacitor/ios
```

4. Create Android/iOS native projects. Adds an android/ios folder.

```bash
npx cap add android
npx cap add ios

# npx cap add electron
```

# Development

1. Start local dev server.

```bash
npm run start
```

2. Build the web files/bundles.

```bash
npm run build
```

3. Sync web code to native project.

-   Copies over your built web bundle to both your Android and iOS projects as well as update the native dependencies that Capacitor uses.

```bash
npx cap sync
```

3. Android studio > run/refresh the app.

# Plugins

They need to be installed before using.

There are official ones...

```
npm install @capacitor/camera
npm install @capacitor/haptics
```

And community ones.

https://github.com/capacitor-community

https://github.com/capawesome-team/capacitor-plugins

Capacitor has support for most Cordova plugins.

https://github.com/danielsogl/awesome-cordova-plugins  
https://danielsogl.gitbook.io/awesome-cordova-plugins

```
npm install @awesome-cordova-plugins/core --save
```

# Testing

You can test the app by:

-   Connecting via cable.
-   Connectin via WI-FI.
-   Emulator.

---

**WI-FI**

Android:

1. Go to pair devices over WI-FI.

2. select the `pair using pairing code` tab.

Phone:

1. Enable Developer options.

    - Go to `About phone > Software information > Build number`.
    - Tap the Build number field 7 times. You will begin seeing a message as you approach the 7 touches.

2. Developer options will now appear in Settings.

3. Go to `Developer options > Wireless debugging`.

4. Tap `Pair device with pairing code` and input the code in Android Studio.

5. Android Studio > run the app.

6. It will show up on your device.

# Compiling (.apk)

Capacitor does not offer a way to build native apps on the command line. Platform-specific CLI tools or an IDE should be used instead.

After syncing, open your target platform's IDE to compile your native app:

-   Android Studio for Android.
-   Xcode for iOS.

**Android**

1. Open `Android Studio`

2. Go to `Build > Build App Bundle (s) / APK (s) > Build APK (s)`

3. Locate .apk in `capacitor-project/android/app/build/outputs/apk/debug`

# Permissions

**Android**

```
./app/android/app/src/main/AndroidManifest.xml
```

# Tutorial

https://youtu.be/1WNYYippEr0?si=Zwowmubp5lCgPSd7

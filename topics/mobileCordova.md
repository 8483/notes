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

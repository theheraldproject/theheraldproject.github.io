---
layout: docs
title: Adding herald to your app
description: Adding herald to your app
menubar: docs_menu
---

# Adding Herald to your app

The process for inserting Herald into your app is similar in concept for both iOS and Android. It involves copying the Herald library from the test app into your app and making the library accessible from your app. This section presents the procedure for inserting the library for iOS and Android.

## Android
1. Open your app project for integration with Herald in Android Studio.
1. Copy the Herald subfolder from the Herald test app project into your project.
1. Open ```settings.gradle``` and insert the following line.

   ```include ':Herald'```
1. Select ```File > Sync Project with Gradle Files```, the Herald module should now appear in your project.
1. Open ```build.gradle``` for your app and insert the following line.

   ```implementation project(':Herald')```
1. Select ```File > Sync Project with Gradle Files```, the Herald module is now available for use in your project. 
1. Copy ```requestPermissions``` and ```onRequestPermissionsResult``` methods from the ```MainActivity``` of the Herald test app into your main app activity, along with the ```permissionRequestCode``` static variable; call the ```requestPermissions``` method on initialisation of your app to ensure all the required permissions are requested and granted for Herald to function.
1. Select ```Build > Make Project``` to ensure Herald is able to coexist with your existing app code, resolve any naming clashes and compilation errors now before proceeding to integration.


## iOS
1. Open your app project for integration with Herald in Xcode.
1. Copy the Herald subfolder from the Herald test app project into your app project in Finder.
1. Drag and drop ```Herald.xcodeproj``` from the Herald subfolder in your project from Finder into Xcode under your project, the ```Herald.framework``` product is now available for use in your app.
1. Open your app configuration, go to ```General > Frameworks, Libraries, and Embedded Content```, then drag and drop the ```Herald.framework``` product to this list to add the framework as dependency.
1. Select ```Product > Clean Build Folder```, then ```Product > Build For > Testing``` to ensure Herald is able to coexist with your existing app code, resolve any naming clashes and compilation errors now before proceeding to integration
1. The Herald framework is now available for use in your app by importing the Herald module in your app code with the following line at the top of your code.

   ```import Herald```

## Future options

In future (v1.2) Herald will be available from common Maven, Gradle, and CocoaPods repositories.

## Now integration Herald to your app

There are [iOS]({{"/guide/ios" | relative_url }}) and [Android]({{"/guide/android" | relative_url }}) application integration pages for this

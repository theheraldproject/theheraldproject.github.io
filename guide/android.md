---
layout: docs
title: Android application integration
description: Android application integration
menubar: docs_menu
---

# Android application integration

Android integration is practically identical to iOS as they share the same API. Please refer to the iOS integration guide for design details. For consistency, the description for Java methods will use the iOS function notation, e.g. ```payload:data``` refers to the payload(Data data) method.

## 1. Implement a PayloadDataSupplier

For fixed length payloads, extend the ```DefaultPayloadDataSupplier``` class to wrap your payload data generator. This class implements the ```PayloadDataSupplier``` interface and offers a default implementation for ```payload:data``` method. The default method calls the payload:timestamp method to obtain an example payload for establishing the payload length for convenience. A production solution that uses a fixed length payload should override this method for efficiency.

For variable length payloads, please implement the ```PayloadDataSupplier``` interface and provide your own parser to separating the raw data (byte array of concatenated payload data) into individual payloads.

## 2. Instantiate a SensorArray

The ```SensorArray``` class constructor requires the application context and a ```PayloadDataSupplier``` as parameters. The former is necessary for access to the device file system (e.g. logging to plain text files) and Bluetooth functionalities. The latter provides the payloads for exchange with other devices. Like the iOS ```SensorArray```, the constructor defines a collection of sensors for proximity detection. At this stage, only the BLE sensor is included in the Android ```SensorArray``` as location services are not required for background operation. A future version will include an optional GPS sensor like iOS for location monitoring if required.

Logging functions are identical to iOS and the output CSV files are also identical in content and format, thus the data can be analysed using common analysis and visualisation scripts across the two platforms. Once again, the optional logging functions include contact, statistics, detection, and battery logs. Like iOS, the ```ConcreteSensorLogger``` class provide all the logging functions across Herald, thus log level is adjustable at ```BLESensorConfiguration.logLevel```. The log files can be downloaded from the device using the Android File Transfer utility (see readme.md file on GitHub repository for details on log data download).

The key differences in the Android constructor are the initialisation of ```ConcreteSensorLogger``` and ```ForegroundService```. The former is required to set the reference to the application context, thus enabling access to the file system for writing log files. The latter is essential for enabling background operation of BLE functions. The foreground service does not perform any actual function, but is a requirement for background operation on Android. The ```ForegroundService``` class call the ```NotificationService``` class to create a visible notification, thus granting the app permission for continuous background operation. These are mandatory requirements on Android. If your app has a constantly visible notification, NotificationService can be disabled in ```ForegroundService``` to avoid showing two notifications for one app.

## 3. Implement a SensorDelegate

The SensorDelegate interface define exactly the same set of callback functions as the iOS ```SensorDelegate```, offering the same logic and data. The ```DefaultSensorDelegate``` class implements all the callback functions in this interface, thus an application can extend this class and override a subset of the callback functions for simplicity. Once again, simple integration involves overriding the ```didMeasure:withPayload``` method to receive distance measurements along with target device payload data.

   ```void sensor(SensorType sensor, Proximity didMeasure, TargetIdentifier fromTarget, PayloadData withPayload);```

All other delegate functions are called under the same circumstances and sequence as the iOS functions. Please refer to the iOS ```SensorDelegate``` description for details.
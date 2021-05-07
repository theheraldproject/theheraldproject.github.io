---
layout: docs
title: Protocol lifecycle
description: Protocol lifecycle
menubar: docs_menu
---

# Protocol lifecycle events

This page details specifics of detection and interaction in different device scenarios.

## Android to Android lifecycle

Android detection of other Android devices follow the standard process presented in the overview. The key differences are as follows.

### BLE capacity

BLE on Android devices is typically more limited in capacity than iOS devices. This means an Android device can maintain fewer concurrent open BLE connections than iOS devices. As such, all BLE connections with Android devices in Herald are short lived connections; they are disconnected as soon as possible to minimise demand on this limited resource.


### BLE stability

BLE on Android devices can become unstable and fail silently if (i) many concurrent connections are held over a long period, or (ii) many connections are made to the device in a short period of time, or (iii) connections are not gracefully and explicitly disconnected regularly. Experiments have shown frequent connections and disconnections (e.g. once every few seconds) can cause a silent failure, whereby the BLE advert remains discoverable and accepts connections, but connections are never actually successful, and data exchange is impossible. The device needs to be rebooted to recover from this failure. Herald overcomes this problem by minimising connection to Android devices. It will only connect to Android devices to read the Payload data once every 15 minutes, i.e. when the Pseudo device address changes. Regular RSSI measurements are taken without connecting to the device, by using device scan instead which simply reads the BLE advert being broadcasted by the Android device to estimate RSSI.


### BLE address

Android devices may change its Bluetooth device address as frequently as once every few seconds (e.g. Samsung A10), dependent on the manufacturer and device model. This will normally cause the BLE central (and therefore Herald) to consider the device as a new device, and therefore initiate the costly service discovery, and read payload tasks in every scan cycle. Experiments have shown this has a major impact on continuity for the device and also other devices in the vicinity, due to the time required to complete these tasks on all devices. The Pseudo device address was introduced to solve this problem on Android devices. The pseudo address is conceptually identical to the Bluetooth address, i.e. they are both the same length, and randomly generated. The only difference is that the pseudo address is allocated and rotated by Herald, and therefore changes at a fixed interval (every 15 minutes) across all Android devices. The pseudo address enables Herald to recognise the same device, despite the change of Bluetooth address, therefore avoiding reconnection and redundant read payload tasks.


### BLE advertising

While all iOS devices, including iPhone 4S, support BLE advertising, many Android devices do not. Analysis of BBC user device statistics, and phone specification data, has shown 99.18% of all UK smartphones (iOS and Android) support BLE, but only 85.88% of UK smartphones with BLE supports BLE advertising. As such, the probability of a UK phone supporting BLE advertising is 99.18% x 85.88% = 85.18%, and the chance of both UK phones supporting advertising in an encounter is 85.18% x 85.18% = 72.56%. This has a major impact on coverage, therefore Herald has been designed to enable detection if either phone supports advertising, thus increasing the chance to 100% - (100% - 85.18%) x (100% - 85.18%) = 97.80%. This is made possible by utilising the signal characteristic, whereby an Android phone that does not support advertising will write its own Payload data to the target device, to achieve the same outcome as read payload by the target device in the normal process. 

For distance measurements, the non-advertising device will obtain RSSI measurements from an advertising target device, using the normal process, and then write the measured value to the target device via the signal characteristic, therefore achieving the same outcome as if the target device was reading the advert from this non-advertising device. By description, this appears to be a complex and time consuming process, but experiments have shown Herald achieves 99.58% continuity when there are 3 Android devices where one is non-advertising, and 99.75% continuity when there are 2 iOS devices and 1 non-advertising Android device. Please note, this solution cannot capture the interaction between 2 non-advertising Android devices; the chance of this occurring in the UK is 100% - 97.80% = 2.20%. An experimental solution was developed and tested, where another iOS or Android device in the vicinity acted as a relay for non-advertising Android devices, however the solution was discounted because it had a negative impact on continuity across all devices.


### Message fragmentation

Writing over 20 bytes of data to the signal characteristic of an Android device will cause message fragmentation, where the data is split into 20 byte blocks for transmission to the target Android device. This also has a side effect of converting a write with response request, into one without any response, thus making it impossible to confirm whether the write was successful. While it is possible to increase the MTU (maximum transmission unit) size of the BLE transmission, experiments have shown the setting to be unreliable, not always supported by devices, and causes compatibility issues with iOS devices. As such, Herald uses its own message fragmentation procedure for Android-Android communication to segment data into 20 byte blocks for transmission and reconstituting the fragments at the target Android device, thus enabling write with response for long messages on Android, while maintaining compatibility with iOS devices.


### Background operation

Full background operation on Android is enabled by registering Herald as a foreground service, and presenting an app notification to ensure the user is aware that Herald is running in the background.


### Location permission

Access to any BLE functionalities on Android requires location permission. The logic is that the rough location of a user can be identified by BLE beacons, which is often more precise than GPS location tracking. As such Herald requires location permission, although GPS location tracking functionality is not implemented in Herald for Android.


## Android detection of iOS

Android detection of iOS devices follow the standard process presented in the overview. The key differences are as follows.

### Scan for peripherals

Background BLE peripheral scanning on Android must include a search criteria that can be applied to the BLE advert for filtering, such as the BLE service identifier or manufacturer identifier. Android devices follow the Bluetooth standard, where the BLE advert includes the service identifier, thus making it easy to scan for Android devices advertising Herald services. While this is also true for iOS devices when the app is in the foreground, the BLE advert changes to a proprietary format when the app runs in the background, which is expected to be the norm. The Apple proprietary format hides the service identifier in encoded form in an overflow area, making it impossible for Android and iOS devices to discover the device by service identifier. 

The only usable information in the iOS background BLE advert is the manufacturer identifier which identifies the device as an Apple device. As such the Android scan for peripheral must search for either a device that is advertising Herald services, or an Apple device. On discovery of an Apple device, Herald will take note of its Bluetooth device address, and attempt connection to perform service discovery, to confirm whether it is offering Herald services. If the service is found, it will handle the device like other Android devices, whereby the Payload data is read from the payload characteristic, and the connection is terminated as soon as possible. Distance measurements are taken without opening a connection, and simply relying on regular scans of BLE adverts to obtain RSSI measurements. 

If the service is not found, the device will be cached and marked with an ignore flag, which expires after one minute. The expiry time increases by 20% for each service discovery failure, up to 3 minutes. This strategy is necessary because service discovery failure may be a transient fault which is common in BLE communication, thus Herald cannot ignore the device on just one failure.


### BLE advertising

Android devices that do no support BLE advertising will write its Payload data and RSSI measurements to the iOS device via the signal characteristic, using the same process as Android-Android communication for non-advertising Android devices.


## iOS detection of Android

iOS detection of Android devices follow the standard process presented in the overview. The key differences are as follows.

### Scan for peripherals

Background BLE peripheral scanning on iOS must include a search criteria for the BLE service identifier. Android adverts always include the service identifier, and therefore Android devices are always discoverable by iOS devices, even when the iOS device is conducting a BLE scan while the app is running in the background. Furthermore, every call to scan for peripherals on an iOS device will report all the Android devices in the vicinity, even when the iOS app is in the background. 

These behaviours on iOS means as iOS device can take regular distance measurements of Android devices by starting a scan at regular intervals, thus avoiding connection to the Android device, unless it needs to obtain Payload data from the device. This reduces power consumption on both iOS and Android devices, and also reduces the chance of causing irrecoverable faults on Android BLE capability on some devices from repeatedly connecting to and disconnecting from the device.

### BLE address

Android devices may change its Bluetooth device address as frequently as once every few seconds, thus a Pseudo device address is included in the manufacturer data region of the Android BLE advert. This is used by iOS devices to avoid redundant communication with the same target device, using the same process as Android-Android communication.

### Background operation

Background iOS operation is challenging in general as iOS prefers to place apps in a suspended state as soon as possible to conserve battery power. BLE advertising and scanning while an app is in suspended state is made possible by iOS CoreBluetooth state preservation and restoration, where iOS takes responsibility of BLE operations while the app is suspended, and the app is restored for a moment (up to 10 seconds) to respond to BLE events. 

Android adverts reliably trigger iOS state restoration when there is a pending scan for peripheral call. In other words, if a scan has been initiated on iOS, and the app enters suspended state, the presence of an Android advert will trigger iOS to restore the app to handle the event. On restoration, Herald will handle the discovery event, and also initiate a pending scan after 8 seconds. Given every iOS scan reports all Android devices, assuming an Android device is within detection range, this process will ensure the app remains in background state to take regular RSSI measurements for the Android device, i.e. the app will not enter suspended state as the pending scan will trigger state restoration before the app is suspended. 

Experiments have shown an iPhone running nothing but Herald will use 1-2% of battery power per hour. (Less than Whatsapp!)


## iOS detection of iOS

iOS detection of other iOS devices follow the standard process presented in the overview. The key differences are as follows.

### BLE advertising

iOS changes the BLE advert to a proprietary format when the app enters background state. The service identifier becomes hidden in an overflow data area, and the data is encoded in a proprietary format. BLE scan by another iOS device will only discover this background advert if the app on the scanning device is running in the foreground, which is not expected to be the normal mode of operation. An undocumented iOS behaviour that was discovered by an open source project enables detection of background adverts while both devices are in background mode. 

Background BLE scan can discover a background BLE advert, if the scanning device has:-

i. location monitoring enabled, 
ii. beacon ranging enabled, and 
iii. the display is on. 

Once the target device has been discovered, a connection can be established and all BLE communication can operate normally in the background using standard mechanisms. This has been implemented as an optional capability in Herald without impacting privacy, where location monitoring is enabled but the resolution is set to maximum (3km) and the callback functions are non-functional, thus location information is not being captured by the app. 

Beacon ranging is also enabled but configured to seek a fictional beacon identifier, and the callback functions are also non-functional. For the display, there are two options, one is to use a local notification to trigger display activity at regular intervals, but this is distracting and tests have shown the method to be unreliable, especially if the device is linked to an iWatch, where the notification appears on the watch instead. 

The second option is to simply rely on normal user behaviour to trigger screen activity. Research (RescueTime, 2020) has shown an average person spends 3 hours 15 minutes on the phone per day, and pick up the phone 58 times per day, 30 of which are during working hours (9am - 5pm). In other words, the phone display is naturally on at regular intervals throughout the day during normal usage, thus regularly triggering discovery of background BLE advert, even when the app is in the background. Herald has been implemented to enable discovery by both iPhones in background mode, if a discovery has been made by either phone, thus the chance of discovery has been doubled to 60 times during working hours (once every 8 minutes).


### Scan for peripherals

Background BLE peripheral scanning on iOS will only report background iOS adverts once. Unlike iOS scan of Android adverts, where every scan call will report all Android adverts, calling scan again will not report a known iOS advert again, even if the iPhone has changed its Bluetooth device address. 

In other words, when both devices are in background mode, iOS will only report the initial discovery of another iOS device, when it is consider a new device, however when the target device has changed its address, making it essentially a new device, it will not be reported again by iOS, and there is no mechanism for detecting the change of address either. 

This behaviour means regular distance measurements can only be taken if an active connection is established between two iOS devices, furthermore if a device goes out of range for a prolonged period (e.g. 15 minutes) and its Bluetooth device address changes while out of range, the connection cannot be resumed by a pending connection call, and the device must be discovered as a new device (i.e. display on to trigger discovery by either device).

### Measure distance

RSSI measurement is reported as part of BLE advert discovery callback, but discovery only occurs once while both apps are running in the background, thus an active connection must be established between the iOS devices to enable regular distance measurements by calling the read RSSI function on the CoreBluetooth API. Herald implements a
 shifting timer that triggers read RSSI for all connected iOS devices every 8 seconds. The timer is primed by a call to any of the BLE delegate functions, thus upon discovery of a target device, a suspended app will be restored by iOS, and this timer will ensure the app runs indefinitely to take RSSI measurements at regular intervals.

### Background operation

iOS state preservation and restoration enables a suspended app to be restored for about 10 seconds to handle BLE events, before returning to a suspended state. Specifically, the BLE events that reliably trigger state restoration are discovery, read, write, and notify. Unfortunately, the response to a read RSSI request is not a reliable trigger. As such, keeping an iOS app running indefinitely, while in the background, to take regular distance measurements requires a “keep awake” mechanism that uses read, write, and notify to prevent the app from entering a suspended idle state. Herald implements a process, where upon discovery of an iOS device, it will establish connection, then initiate service and characteristic discovery to identify the signal characteristic, which supports write and notify in Herald for iOS (on Android it is write only). The device will then subscribe to this signal characteristic and keep the connection open, thus any change to the characteristic value will trigger state restoration via a BLE write callback. Considering both iOS devices have subscribed to each other’s signal characteristic, they can keep each other from entering a suspended state by writing to this characteristic, which in turns triggers a notify call to all subscribers, and priming the next write request, thus making the iOS apps run indefinitely in background mode. Herald implements this process by writing empty data to the signal characteristic after a short delay (up to 8 seconds) when it receives any BLE callback. Experiments have confirmed iPhones stay connected and awake indefinitely to take RSSI measurements at regular intervals, after the initial discovery, achieving 100% continuity in tests lasting over 20 hours.

### Calling card

Location permission is mandatory on Android devices for access to BLE services, but it is optional on iOS devices up to iOS 14.1. iOS 14.2 (introduced November 2020) requires the fine grained location permission if using Bluetooth in the background. This brings iOS permissions in line with Android permissions for the first time. 

Enabling location permission on iOS for Herald has no impact on privacy or security, and it will enable direct iOS-iOS discovery while both devices are in background mode. However, if this is unacceptable for the population, or reliance on normal phone usage to tigger screen on events (and therefore device discovery) is insufficient, Herald implements a “calling card” mechanism, where an Android device in the vicinity acts as a relay for iOS devices, to enable discovery and regular RSSI measurements, even if the iOS devices have not discovered each other. 

This mechanism relies on the Android device to discover iOS devices in the vicinity and take RSSI measurements for each device, as per normal Android-iOS processes. The Payload data of recently discovered iOS devices, and RSSI measurements captured by the Android device are then written to all the iOS devices’ signal characteristic as data at regular intervals, thus making all iOS devices aware of each other even without a direct connection between these devices, and also enable distance estimation based on their relative distance to the Android device. 

While the distance between iOS devices, estimated via the Android device is not as accurate as that taken from a direct connection, analysis has shown it is statistically similar if the devices are moving in random directions, and sufficiently accurate for contact tracing. Herald manages the data being shared by the Android device to each iOS device to minimise repetition. Connection from Android to iOS for sharing Payload data and writing RSSI measurements are short lived, and the frequency of data write is scheduled to minimise impact on continuity. 

Experiments have shown this mechanism, when applied to two iPhones with location permission disabled, and using a normal Android device as relay achieves 97.05% to 97.40% continuity; tests involving a non-advertising Android device achieved 99.75% continuity. Formal testing with 10 devices (5 Android, 5 iOS with location disabled) showed the solution achieved 93.06% continuity. All the tests ran for 3 to 12 hours.

Next: [Summary]({{"/design/summary" | relative_url }})
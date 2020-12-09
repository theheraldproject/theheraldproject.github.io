---
layout: docs
title: iOS application integration
description: iOS application integration
menubar: docs_menu
---

# iOS Application Integration

## 1. Implement a PayloadDataSupplier
Contact tracing apps enable the exchange of a globally unique device identifier between devices for contact logging. The encoding of device identifier into binary data is usually a key element in the security and privacy design, thus Herald will simply act as a transport for your binary data to avoid changing the fundamental designs elements of your app.

The PayloadDataSupplier protocol is the API for integrating your device identifier payload. The protocol has two functions. The first function takes a timestamp and returns the binary payload data for transmission to another device. This function is called upon every device identifier exchange for all devices, thus making it possible to mutate the payload on every exchange and encrypt the payload based on time. For reference, ```PayloadTimestamp``` and ```PayloadData``` are simply type aliases for Date and Data. Herald supports both fixed and variable length payloads up to 510 bytes. The performance reported in literature is based on a 129 byte payload.

   ```func payload(_ timestamp: PayloadTimestamp) -> PayloadData```

The second function is used to parse raw data into a list of payloads. This function is required because Herald uses Android devices to relay payloads between iOS devices as one of the two mechanisms for enabling iOS background detection. Internally within Herald, upon discovery of an iOS device (A), it will concatenate several recently seen iOS device payloads (e.g. B,C,D) and write the concatenated binary data to the iOS device (A). This parser function is then called at (A) to retrieve the separate payloads (i.e. B,C,D), thus enabling A to discover B,C,D while they are all in background mode. Please note, Herald also includes a second mechanism that enables direct detection between iOS background devices without relying on an Android device.

   ```func payload(_ data: Data) -> [PayloadData]```

Implementation of this second function is optional for apps that use a fixed length payload, as a default implementation (see extension to ```PayloadDataSupplier```) calls the first function to obtain an example payload to establish the payload length for partitioning concatenated data into individual fixed length payloads. For variable length payloads, you will need to implement your own logic for partitioning concatenated payload data, e.g. by recognising start and end byte patterns. If this is too challenging for your payload format, the Android code can be modified to share on payload at a time instead.

## 2. Instantiate a SensorArray

Herald has been designed as a sensor array for proximity detection, rather than an exclusively BLE based solution. A sensor array combines multiple different technologies for recording person-to-person and person-to-environment encounters, i.e. BLE and ultrasound for encounters with people, and GPS and static beacons for encounters with places. The current implementation includes BLE and GPS sensors. Work is underway for investigating the feasibility of ultrasound, static beacons, and other sensors.

The SensorArray class is initialised with the ```PayloadDataSupplier``` implemented in the previous step. Once initialised, the SensorArray will be activated and deactivated according to Bluetooth state, i.e. proximity detection will commence or cease upon Bluetooth power on or off. For clarity, if Bluetooth is already on when the SensorArray is initialised, proximity detection will be immediately active, and contact events shall be distributed to all registered SensorDelegates. For initial integration testing, given two devices, the application log should show your payload being exchanged across the devices.

### Sensor selection

The initialisation code in SensorArray defines the sensors that make up the sensor array and also the loggers for writing sensor data to files on the local device. The current implementation includes the BLE and GPS sensors. The former is essential for proximity detection with BLE. The latter is required to enable iOS background detection in isolation without relying on Android devices. The GPS sensor is currently configured as a beacon ranging location sensor that is seeking a fictional beacon; this enables iOS devices to discover each other while both devices are running in the background, when the screen is lit, even for an instant.

Bluetooth behaviour on iOS devices is influenced by screen ON events, where background adverts are fully read by a background scanner only when the screen is ON. Considering the average time between device pickup (screen ON) is only 10 minutes during the working day, this offers a reliable secondary discovery method for backgrounded iOS devices when there are no Android devices in the vicinity. Please note, ConcreteGPSSensor can be configured as a complete location monitor by adjusting the desired accuracy and distance filter to enable recording of person-to-environment encounters. The current default parameters essentially disables location monitoring while maintaining location update functionality. The combination of beacon ranging and location update is necessary for enabling background detection.

### Logging

Several loggers have been included in the ```SensorArray``` initialisation code. These were used during development to automate testing and analysis of efficacy, by measuring and logging detection, continuity, and sampling rates in plain text CSV files on device for download and processing by analysis scripts. The ```ContactLog``` logger offers comprehensive recording of all the detect, measure, read, share and visit events and associated data for all encountered devices.

Details about the different events can be found in the ```SensorDelegate``` section. The contacts.csv file generated by this logger contains the time for all distance measurements in raw RSSI for all encountered devices (measure events), and also the mapping between ephemeral device identifiers and payload data (read events), thus making it possible to establish exposure duration and proximity for all encountered devices. The raw data is used during formal testing for measuring detection and continuity rates for assessing efficacy.

The ```StatisticsLog``` logger offers summary statistics (i.e. count, mean, standard deviation, min, max) for the sampling rate, in elapsed time between samples, for each encountered device. The statistics.csv file generated by this logger provides the raw data for estimating completeness, which in simple terms is the difference between the actual start and end time of an encounter and the recorded times. Accurate exposure timings enable accurate infection risk estimation, and this is particularly important when the epidemiological risk model is based on cumulative exposure.

The ```DetectionLog``` logger records key information about the device (i.e. name, model, operating system version), its own payload and all the payloads that it has detected. For formal testing, a  test PayloadDataSupplier has been created for publishing a fixed and consistent payload for each device (see identifier function in AppDelegate), thus the detection.csv file generated by this logger provides the raw data required for measuring detection rate, i.e. did the device detect all other devices in the vicinity.

Finally, the ```BatteryLog``` logger records power source and battery level data at 30 second intervals to measure total power usage of the device over time. The battery.csv file provides a continuous log for assessing power drain.

All four loggers in the ```SensorArray``` initialisation code are optional in a production app. In addition, Herald uses ```SensorLogger``` for detailed logging across all the code to a plain text file log.txt for debugging and analysis. This is also optional in a production app. Log level can be adjusted by changing ```BLESensorConfiguration.logLevel```.

## 3. Implement a SensorDelegate

All sensor events are distributed to all sensor delegates that have been registered with the SensorArray via the add:delegate function. The SensorDelegate protocol includes a set of optional callback functions for receiving event information. For integration, the simplest option is to implement a single high level API function for receiving regular distance measurements along with payload data. This function is called immediately for each measurement taken for every device, where the payload data has already been acquired by Herald.

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier, withPayload: PayloadData)```

The sensor parameter will always be BLE given the SensorArray is only using BLE for proximity sensing. The ```didMeasure``` parameter contains the distance measurement, which is the raw RSSI value. The fromTarget parameter can be ignored as it contains the ephemeral device identifier which has no meaning outside of Herald. The withPayload parameter is the payload data for the device. This is the encoded device identification data for processing by your app.

n short, the didMeasure and ```withPayload``` parameters provide the data for infection risk estimation. A timestamp is not included in the callback function, as it is called immediately for every distance measurement, thus the time of function call according to the time measurement method in your app is the timestamp. It is anticipated that this function will feed the contact log in your app. The raw data provides the basis for identifying contacts and estimating exposure level according to your RSSI calibration data and risk model.

### Other delegate functions

In Herald, a device has an ephemeral identifier (i.e. BLE device address) and an actual identifier  (e.g. registered permanent device identifier for contact tracing) encoded in the payload data. The former is part of the BLE specification and it is used in Herald for identifying physical devices and caching payload data to avoid frequent connect and read requests for efficiency. Please note, this address will change at a variable rate on all devices, and it can rotate as quickly as once every few seconds on some devices (e.g. Samsung A10). The impact of frequent address change has been compensated in Herald by including a rotating pseudo address in the advert on Android devices.

In addition to the high level API function for integration, the ```SensorDelegate``` protocol offers a set of low level callback functions for receiving all the detailed detection events. The benefit of using this low level API is that all data is available, thus enabling the production of a more complete detection timeline for risk assessment.

For instance, Herald will start taking regular distance measurements for a target device upon detection, but there could be a short delay before the payload is successfully read for the device. The high level API will only start reporting distance measurements after the payload is read, whereas the low level API enables retrospective attribution of payload to historic distance measurements based on the ephemeral identifier, thus offering a more precise start time for an encounter and also additional distance measurements for risk estimation. In practice, this is unlikely to change the overall risk estimate given the payload is typically read long before the target device reaches close proximity.

The typical sequence of events from initial device detection to regular distance measurements is as follows:

- New device comes within detection range, this will generate a didDetect event for the device with an ephemeral target identifier.

   ```func sensor(_ sensor: SensorType, didDetect: TargetIdentifier)```
- Regular distance measurements shall be taken upon detection, this will generate a series of didMeasure events for the device to provide raw RSSI measurements. At this point, the measurements are unattributed, as the target identifier is not the payload data, thus there is insufficient information for contact tracing or risk estimation, i.e. we know the device has been near another device, but we don’t know the registered device identifier for attribution.

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier)```
- Herald will attempt to establish connection between the two devices to acquire the payload data. Upon successful connection and data acquisition, it will generate a didRead event to enable association of target identifier with payload data for attribution of historic and future distance measurements.

   ```func sensor(_ sensor: SensorType, didRead: PayloadData, fromTarget: TargetIdentifier)```
- Once the payload of a target device has been acquired, all future distance measurements will generate a didMeasure event followed by a didMeasure:withPayload event.

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier)```

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier, withPayload: PayloadData)```

The ```SensorDelegate``` protocol also offers callback functions for reporting sensor status and location updates (if enabled). A state change for any of the sensors in the sensor array will result in a ```didUpdateState``` event, where the first parameter describes the sensor that changed state and the second parameter describes the new state. State on means the sensor is active and fully operational (e.g. Bluetooth is powered on). State off means it is available on the device but inactive (e.g. Bluetooth is available but powered off). State unavailable means the sensor is either unsupported on the device or no longer working (e.g. Bluetooth is unsupported).

   ```func sensor(_ sensor: SensorType, didUpdateState: SensorState)```

Location sensing enables person-environment (rather then person-person) contact tracing, to isolate people who may have been in contact with contaminated surfaces (e.g. at an open air market), or a dense crowd of people containing infectious individuals (e.g. music festival where detection rate may not be 100% due to density and people not carrying their phones). The current implementation includes a GPS sensor that can be configured for fine grained location monitoring (disabled by default). A future implementation may include other location sensors such as static beacons. All location detections will generate a didVisit event where the location parameter can be a GPS coordinate or place description depending on the sensor.

   ```func sensor(_ sensor: SensorType, didVisit: Location)```

The last remaining callback function in the SensorDelegate protocol is the didShare function. This is unlikely to be used by any app, but it has been included to make all internal data available for evaluation and analysis. As mentioned before, Herald has two mechanisms for enabling iOS background detection, one of which relies on relaying payload data via an Android device in the vicinity. When an Android device shares the payloads of recently seen iOS devices via characteristic write to an iOS device, it will generate a didShare event. The sensor parameter will be BLE as payload sharing only occurs in the BLE sensor. The ```didShare``` parameter is a list of payloads extracted by the payload:data function in ```PayloadDataSupplier```; these are the payloads of recently seen iOS and non-transmitting Android devices, that were shared by an Android device in the vicinity. The target identifier parameter is the ephemeral identifier of the Android device that shared the data.

   ```func sensor(_ sensor: SensorType, didShare: [PayloadData], fromTarget: TargetIdentifier)```

This didShare callback function is only of interest to apps that would like to understand the exact provenance of payload data, e.g. to understand the level of reliance on Android devices for iOS detection. Every call to this function is followed by a series of didMeasure calls for each shared payload. For simplicity, consider two iOS devices (A,B) and one Android device (C). Let’s also assume the Android device (C) has already detected and read payload of A and B, thus generated the ```didDetect```, ```didMeasure``` and ```didRead``` calls. The Android device C knows A and B are iOS devices, thus it will share the payloads of recently seen iOS devices to A and B. In other words, it will share the payload of B with A, and vice versa.

The typical sequence of events at A when C shares the payload of B is as follows:

- Android device (C) shares the payload of iOS device (B), this generates a ```didShare``` event, where the list of payload data contains the payload of B, and the target identifier is the identifier of C as that is the origin of the shared data.

   ```func sensor(_ sensor: SensorType, didShare: [PayloadData], fromTarget: TargetIdentifier)```

- Internally, payload sharing data includes both payload and RSSI data. More specifically, the distance (RSSI) between C and A as measured by C is transmitted to A along with the shared payloads. As such, A can infer its distance to the shared device B based on the distance between A and C. With this in mind, for each shared payload, Herald will generate a series of ```didRead``` and ```didMeasure``` events.

The didRead event offers the individual payload, as if A detected B directly (rather than via C). The target identifier in this instance is a generated ephemeral identifier for B (as the device address is unknown). For information, if the device address becomes available in the future, it will be used in this call.

The didRead event is followed by a didMeasure event, reporting the RSSI value between A and C as approximation for the distance between A and B. Once again, the target identifier is the same as that in the didRead event. Finally, a ```didMeasure:withPayload``` event is generated which combines the information in the previous two events for ease of integration.

   ```func sensor(_ sensor: SensorType, didRead: PayloadData, fromTarget: TargetIdentifier)```

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier)```

   ```func sensor(_ sensor: SensorType, didMeasure: Proximity, fromTarget: TargetIdentifier, withPayload: PayloadData)```

You're now complete with integration! See the Android Herald demo app for an example of all of the above.

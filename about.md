---
layout: docs
title: About Herald
description: About Herald
menubar: about_menu
---

# About the Herald Project

The Herald Proximity project exists to provide a range of
fundamental API to allow software developers to quickly build
applications that rely on regular distance proximity calculation
and the exchange of data between two or more devices.

## Applications

There are a range of use cases, but the most common are below:-

- [Digital Contact Tracing (DCT)]({{"/applications/ct" | relative_url }})  - Herald provides a very reliable DCT system with the widest device support in the world, including dedicated wearables
- Venue check-in - The Herald Venue Check-in Beacon provides a very low cost, automatic, and privacy-preserving alternative to manual QR code scanning in restaurants, bars and other venues. Useful as an aid to Digital Contact Tracing (DCT)
- Visitor services - Provide visitors with their position on your site and additional services like navigation. Very useful for complex University campuses or Hospitals. This is done in a privacy preserving way without sharing their data to the venue.

Read about all potential applications on our [Applications page]({{"/applications" | relative_url }}).

## Why Herald?

Herald provides a number of innovations and features to make it ideally suited to Proximity detection applications.

<a name="phone-support"></a>

### Wide phone support

Herald works on older phones and operating systems. You don't have to own a $1000 smartphone to benefit from Herald.
In order to support at-risk communities, the developing world, and those younger and older people without
access to smartphones, Herald also supports low-cost wearable designs.

We estimate over 98% of smartphones worldwide can use Herald. We support mobile operating systems going all the way back to 2010 when Bluetooth Low Energy first appeared.

<a name="distance"></a>

### Regular distance readings

Herald currently supports Bluetooth Low Energy and will log RSSI readings for other nearby devices for
every single advertisement seen. This allows us to provide much more accurate RSSI readings, and thus
distance conversion, than other protocols. We are also engaged in active research to improve distance
estimation accuracy. Herald can be extended to support other mechanisms such as UWB and high frequency
audio distance estimation all within the same API, with no changes to your applications.

<a name="detection"></a>

### Detect nearby devices

Herald detects 100% of other nearby devices running Herald. This typically takes around 2.5 seconds.
This works whether the devices are in the foreground or background.

<a name="environment"></a>

### Log other readings

In digital contact tracing multiple algorithms can be applied to estimate COVID-19 and other disease
exposure risks. In other environments such as worker safety beacons may be deployed that log and share
other environmental readings such as noxious gas, carbon monoxide, temperature and other readings.
Herald can be used for health monitoring wearables too, provide a rich set of data to individuals and their
doctors.

<a name="interoperability"></a>

### Interoperability

Herald has actively created an interoperability standard that is being contributed to standards body work
worldwide for Digital Contact Tracing interoperability. Whether DCT or another application, all Herald
proximity API capable applications can interoperate at the Bluetooth Low Energy layer allowing the 
same protocol to support multiple proximity based applications and allowing different systems from
different vendors or opensource teams to interoperate on day 1.

## Opensource

All Herald output is licensed under permissive open source licenses.
This allows our work to be used by anyone for commercial or non-commercial purposes.

Herald software is licensed under the Apache 2.0 license.
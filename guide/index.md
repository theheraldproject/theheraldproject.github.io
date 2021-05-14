---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Developer and Integration Guide
description: Full development guide for the iOS and Android Herald protocol and provided sample payloads
menubar: docs_menu
---

# Design & Integration Guides

This section describes how to integrate Herald in to your own applications.

Below is a set of options for using the [Herald Protocol]({{"/protocol" | relative_url }}) in both a Contact Tracing application, or a custom
commercial application.

![Protocol and Payload layers diagram](../images/ProtocolStack.png)

With Herald you can use our suggested full or header [payloads]({{"/payload" | relative_url }}), or an entirely custom payload:-

![Herald Payload Contents](../images/Payloads.png)

## Overview

Herald is a cross-platform proximity detection solution for iOS and Android mobile phones, low-cost wearables, and beacons. It detects all devices within epidemiologically relevant range (8 metres), and offers frequent sampling of distance measurements (at least one sample per 30 seconds) for each device. The solution has been designed to operate indefinitely on screen locked iOS and Android devices in the background without any user interaction. It overcomes all the background operation challenges, especially on iOS devices. Herald works on iOS 9.3+ and Android 5.0+, making it compatible with 98% of smartphones worldwide. The solution is also payload agnostic, to facilitate integration with existing contact tracing apps, acting as a reliable cross-platform transport for existing device identification data payload, thus it has no impact on approved security and privacy designs to minimise disruption to existing apps.

This integration guide aims to offer an easy to follow recipe for enhancing existing contact tracing apps with Herald. As an overview, the process involves:

- Isolating the existing device identification data payload generation code and wrapping it into a PayloadDataSupplier.
- Isolating the existing contact data logging code and wrapping it into a SensorDelegate.
- Remove existing Bluetooth Low Energy (BLE) code for detecting devices, exchanging device identification data, and distance measurement.
- Replace the existing BLE code with the Herald SensorArray for the PayloadDataSupplier and register the SensorDelegate for receiving detection and distance measurement events.

The API for both iOS and Android have been kept nearly identical in concept and in code where possible for ease of integration. The integration process can be as quick as one working day for both platforms; this has been tested on real open source apps. The API design also considered the architecture of several public Contact Tracing apps for ease of integration.

See the integration guide page navigation to the left for each section.

Start here: [About the code]({{"/guide/code" | relative_url }})

## Something missing?

Please do [log an Issue on GitHub](https://github.com/theheraldproject/theheraldproject.github.,io/issues) for any missing documentation
or to ask us to correct errors or provide clarification. We want to provide very high quality integration
documentation to help development teams integrate the Herald API.
---
layout: docs
title: Protocol Specification
description: Protocol Specification
menubar: reference_menu
---

# Herald Protocol Formal Specification

This page formally specifies the Extended Data Payload. This is referenced in both the
[Secured Payload Specification]({{"/specs/payload-secured" | relative_url }}) and the
[Beacon Payload Specification]({{"/payload/payload-beacon" | relative_url }}).

This specification is **IMPLEMENTED** in Herald v1.0 and above.

## Modal verbs terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL 
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

Note that these terms apply whether they are uppercase or lowercase
and no matter what styles are applied to the phrases.

## Executive summary

The Herald Protocol is a layer 7 (application) layer protocol in OSI Model terms which 
uses a subset of standard Bluetooth specification features in order to provide reliable
Bluetooth communication between devices.

Herald was designed to perform this originally for Contact Tracing applications but
the standard presented here can be used for any regular, unsupervised interaction
of applications that run on consumer devices, wearables or beacons for a variety of use cases.

The Herald Protocol specifies the reliable data transfer mechanisms available whereas
Payload standards specify the application specific data payloads that can be transported
via the Herald Protocol. These Payloads are abstracted away from the protocol to provide
for version interoperability and allow for future changes without disruption.

## Introduction

The protocol builds on Bluetooth Low Energy 4.0 using the minimum number of features
necessary to provide for reliable detection, payload exchange, messaging, and frequent
RSSI readings.

## Scope

### What is in scope

- iOS, Android OS, and Wearable (nRF Connect SDK) devices
- Bluetooth Low Energy 4.0 and above support

### What is out of scope

The following items are not considered within this standard:-

- Payload data format, other than maximum size per exchange
- Data format of the immediate send message, except its preamble
- Bluetooth Mesh gateway interoperability (A separate standard to be defined in future)

## References

### Normative references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are necessary for the application of the present document.

Adam Fowler, 2020. A Fair Efficacy Formula for comparing Contact Tracing applications. medRxiv. See HP-4 below.

### Information references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are not necessary for the application of the present document but they assist the user with regard to a particular subject area.

NONE

## Definition of terms, symbols and abbreviations

Below are technical terms and abbreviations used throughout this document.

### Terms

**Beacon** - A fixed device in a location that provides location-specific information. May be called a Bluetooth Beacon.

**Big Endian** - Also known as 'Network Order'. A way of encoding numeric values for transport over data networks. Bluetooth (and most network protocols) are Big Endian.

**Herald Project** - The opensource contributors and maintainers of the Herald website, code, and standards documents.

### Symbols

None.

### Abbreviations

**App** - Short for Application. Usually a 'mobile application' on a phone or 'wearable application'

**BLe** - Bluetooth Low Energy. Introduced as part of the Bluetooth standard in Bluetooth 4.0 in 2010. As of this writing the latest version was Bluetooth 5.2. Versions are backwards compatible. (I.e. a Bluetooth 5 device can communicate with a Bluetooth 4 device, and vice versa).

**COVID-19** - Common name referring to SARS-CoV-2.

**ECIES** - Elliptic Curve Integrated Encryption Scheme. A modern encryption scheme believed to be resistant to emerging quantum computing encryption brute forcing mechanisms.

**GATT** - Bluetooth Generic Attribute Profile.

**iOS** - Apple's Operating System for mobile phones

**MAC or MAC address** - Media Access Control address. A short (6 byte in Bluetooth Low Energy) approximately unique identifier for a device on a network, such as Bluetooth Low Energy, before any high-level identity or security information can be ascertained. Typically rotates every 15 minutes on modern phones. May be static on beacons.

**RSSI** - Received Signal Strength Indicator. A signal strength approximation calculated on a device based on the power of a remote transmission. Calculated by the Bluetooth chipset on the local device, and not transmitted. The scale varies by manufacturer, some use 0 to 255, others 0 to 100. Normally expressed as a negative integer rather than a positive number.

**OS** - Operating System. Typically refers to a mobile phone or Bluetooth beacon operating system. The software on which a Herald based Bluetooth app operates. Operating Systems typically provide a unique API for communicating with that device's Bluetooth capabilities. Some functionality in OS' Bluetooth layers may be hidden from the app developer, or non-standard.

**SIG** - Typically Bluetooth SIG. Standards Interest Group. Group of technolists that decide on changes to the Bluetooth standards. See HP-3 below.

**TxPower** - Transmit power. Expressed in integer or floating point in dBm. Known to the transmitter only, unless 'advertised' in the Bluetooth GATT field for TxPower. As it is transmitted it must be presumed that it could be spoofed by the sender.

**UUID** - Universally Unique Identifier. Typically a 'Version 4 UUID'. A 128-bit (16 byte) identifier that unique identifies a service or characteristic in Bluetooth. A shorter (4 byte) 'short code' identifier may instead be registered with the Bluetooth SIG.

## Protocol Design Constraints

The below are the constraints placed on the design of the system that the rest of this standard is defined within.

Many of those at risk of COVID-19 themselves, and of spreading it throughout working as key workers, may
be low paid. This means they are disproportionately more likely to own older devices. For this reason
the Herald protocol MUST support iOS 9.3 and above and Android 5.0 and above.

The use of any API that is only supported by later operating systems in an app that is designed
to use herald and the impact on availability on older devices SHOULD be weighed against the
benefits of those API. For this reason Herald does not use ECIES encryption, although application
developers using Herald MAY choose to use this in their applications.

## Protocol Lifecycle Summary

There are several stages to a 'contact event' whereby two phones discover each other:-

1. Phone A with a more powerful transmitter has their Bluetooth advertisement seen by Phone B (one way detection)
1. Phone A and B are both in range and MAY discover each other. Two way communication is initiated for payload exchange.
1. Whilst within range the Bluetooth adverts allow an RSSI (signal strength) reading to be taken. These are calculated locally on each device
1. The devices begin to move apart, and one way detection again happens
1. Both phones move out of range. The contact event end time is that of the last RSSI reading for the other device.

**NOTE:** Although Bluetooth has the concept of a 'connection', Herald may not keep this connection always open.
Even when a Bluetooth connection is established, if two phones move out of range then there is no 'connection closed'
event. The Herald protocol MUST take this in to account in its lifecycle stages.

## Discovery and identity

Advertising is used for discovery. For devices that do not support advertising, a write characteristic is provided so the device can tell
another advertising device that it is present. Herald indentity is encapsulated in the data payload. The payload mechanism is pluggable.

The same Herald protocol device MAY have multiple Bluetooth Low Energy MAC addresses over time. The device MAY change identity payload at the same
frequency or more regularly than the MAC address rotates.

A Herald protocol compliant device MUST provide scanning for advertisements and SHOULD provide support for advertising GATT services and characteristics itself.

Herald supports old devices which MAY NOT support advertising. To avoid one-way detection, a Herald protocol compliant device that does not support advertising 
MUST support writing its own payload (and thus its view of the signal strength for the contact) to the device that is advertising. This is the digital
equivalent of tapping someone on the shoulder to let them know you are there.

A Herald protocol device that supports advertising MUST provide a payload characteristic to be read. This characteristic MAY also be notifiable, and SHOULD be
notifiable for iOS devices.

A Herald protocol device that supports advertising MUST provide a writeable payload characteristic for devices that themselves do not support advertising. This 
Characteristic MUST support 'write with response' and NOT 'write without response'.

A Herald device MUST NOT rely on the underlying Operating System's view of whether a peripheral is 'connected' or 'disconnected' and MUST implement
its own connection attempt timeouts to workaround bugs in the OS' Bluetooth subsystems.

**NOTE**: iOS Core Bluetooth does not correctly update Peripheral state from 'connecting' to 'connected' reliably.

### Device discovery introspection

This section details any non-standard discovery mechanisms required due to device manufacturers deviation from the Bluetooth Specification.

Some Android devices incorrectly rotate their BLe MAC address every few seconds. Device which do this MUST provide a Pseudo Device Address within
the manufacturer data area (manufacturer code 65530 or 0xFFFA big endian). This Pseudo Device Addres SHOULD rotate with the same time as the BLe MAC
address changes. 

**NOTE** Not all Operating Systems provided a callback to detect this which is why this item is SHOULD instead of MUST. Best efforts SHOULD be made to
closely match the BLe MAC address rotation frequency in order to minimise the ability for long term tracking using the Pseudo Device Address.

A Pseudo Device Address MUST be a 6 byte value that cannot have its future values reasonably be deduced from a randomly intercepted (NOT just first value generated in a static test) value. 

A Pseudo Device Address MUST NOT be used on devices whose Operating Systems provide a reliable MAC address over a period of minutes.

**NOTE**: Pseudo device address prevents constantly data payload exchange due to a device being seen as a 'new device'. It is not needed on OS that provide
reliable MAC addresses for minutes. It is also not required on iOS where the Peripheral ID provided by iOS is constant even if MAC address changes.

Herald protocol devices SHOULD introspect advertising data for devices with non standards compliant service advertising in order to reasonably detect and connect
to a Herald service available on the phone. This mechanism will vary from manufacturer to manufacturer.

**NOTE**: iOS devices do not obey the Bluetooth specification (See HP-1 below) as when their screens are off (i.e. the phone is 'backgrounded') it uses a proprietary, not publicly documented,
way to highlight the app level services available on the device.

## Session and distance estimation concepts

The Herald API will maintain a current state of a device in terms of Herald discovery, Herald protocol and payload support, and its recent successful communications.

A contact event starts when a new device is first detected via its advertisement. A contact event ends when the last RSSI value was attributed to a known Herald identified device.

A Herald Protocol compliant device SHOULD minimise the number of times data payload is exchanged, and preferably not repeat the exchange of the same data payload to the same device.

A connection MUST be opened to transfer data payloads between devices. This connection SHOULD be closed as soon as all necessary activity is completed. A connection MAY be maintained
if the Operating System of a device does not correctly relay ongoing advertisement packets to a Herald protocol device.

A Herald Protocol application MUST perform a Bluetooth Low Energy advertisement scan at least once every thirty seconds. The application SHOULD complete all outstanding interaction
with those devices as soon as possible after discovery.

**NOTE**: Due to a bug in iOS Core Bluetooth confirmed by Apple in June 2020, iOS does not correctly pass advertisements for other iOS devices
to background Bluetooth applications that have subscribed to advertisements with a specific Service UUID. This was confirmed as a bug and is not a privacy feature.

Where connections are needed to be kept open to correctly maintain state, if the simultaneous connection limit is reached then the oldest connection SHOULD be dropped so new
devices are prioritised for detection and distance estimation. Herald protocol based apps MAY choose instead to use RSSI to prioritise nearby devices instead.

Distance Estimation for Bluetooth MUST pass all advertisements' RSSI readings to the Herald app. This app MAY use an intermediate helper class to average, blend, or otherwise
attempt to create an accurate distance estimation in metres from multiple of these raw RSSI notifications.

**NOTE**: Herald v1.2 has been observed on older phones to provide over 140 000 RSSI readings on a single phone over a 23 hour period.

Where TxPower is provided in advertisement data it MUST be provided to the app via a distance estimation callback on each change.

**NOTE**: For most consumer mobile devices, even in battery saving mode, TxPower tends to be static and not fluctuate much.

A Herald protocol app MAY be configured to introspect a device via its Device Info attribute service in order to 
discover the Device Model.

**NOTE**: All modern iOS devices provide the model of the iPhone and iPad via an unauthenticated characteristic (E.g. 'iPhone9,3' for the iPhone 6) that can be read
without the owner of the device being aware or notified. This information is useful for calibrating distance estimation and converting from RSSI, TxPower and model
to distance.

A Herald data payload MAY incorporate additional information useful for accurate distance estimation. This COULD include
non-Bluetooth derived data including approximate location (E.g. postal district) or high frequency sound distance estimations in metres.
This MAY also include accuracy information of RSSI (E.g. +/- RSSI recent reading variability) or sound readings (E.g. +/- accuracy of reading in metres).

**NOTE**: See the 'Herald Extended Data Payload' specification for examples.

## Out of sequence requests

Some applications MAY need to exchange synchronisation, calibration, or discovery messages between devices. These MUST be supported for sending
immediately and out of sequence from the main discovery, payload identity reading, and distance estimation cycle.

An immediate send payload MUST use the signal (write) characteristic to exchange data.

A Herald protocol device MUST send this data as the first (highest priority) next action to the target device(s). This device MAY attempt to
open a new connection if the target device is not currently connected. A Herald app MAY send the immediate send request over an existing connection.

A Herald protocol device MAY send such a message to 'All' nearby Herald devices. A Herald protocol compliant app MAY device how to select targets.

**NOTE**: On the Herald default protocol implementation, Herald attempts to send the message to all devices who have been confirmed as Herald
protocol devices but which also have a 'last updated' timestamp (i.e. last payload exchange or RSSI reading) within the last 60 seconds.

## End of a contact event

A contact event ends when the last RSSI value was successfully read.

A contact event MUST NOT end when the Bluetooth payload exchange connection is closed.

**NOTE**: Bluetooth is a radio based protocol. All these protocols are fundamentally unstable and close connections regularly
for a variety of reasons. This is normal behaviour.

An app using Herald derived data MAY choose to extend the end time of a contact event by a well defined period of time. This MAY,
for example, be the mean time between device scans. This time MUST be a maximum of 30 seconds.

**NOTE**: The Fair Efficacy Formula (See HP-4 below) paper contains the rationale for not artificially assuming large (longer than 30 seconds is large)
extensions to times for a contact event. Some time extension MAY be needed in situations where only a single RSSI reading was exchanged,
as otherwise the contact event would be 'zero seconds' and represent zero risk.

## Bluetooth identifiers

The below are the identifiers being used for Herald compliant apps.

An app provider MAY choose to use their own Service UUIDs. Such apps WILL NOT be provided with
international and device interopability via the Herald International Interoperability standard that is
documented separately.

### Manufacturer ID

Herald is currently using a manufacturer data area ID of 65530 (0xFFFA big endian) for the Pseudo Device Address.
The Herald Project MAY change this to one registered with the Bluetooth Standard SIG.

**NOTE**: See HP-2 below for the Bluetooth SIG registered Manufacturer IDs list

### Service ID

Herald uses a long Service UUID value (16 byte) of 428132af-4746-42d3-801e-4572d65bfd9b.

The Herald Project MAY change this to a registered short (4 byte) value registered with the Bluetooth Standard SIG.

**NOTE**: See HP-2 below for the Bluetooth SIG registered service IDs list

This service MUST be advertised as a primary service.

### Payload Characteristic

The Characteristic UUID for the data payload characteristic is 3e98c0f8-8f05-4829-a121-43e38f8933e7

This characteristic MUST be advertised as a Read characteristic with read permission enabled.

### Write (Signal) Characteristics

Two characteristics are used - one for Bluetooth compliant devices (legacy name of 'Android signal characteristic')
and one for non-compliant devices (legacy name of 'iOS signal Characteristic').

The compliant signal characteristic MUST have a UUID of f617b813-092e-437a-8324-e09a80821a11.

The compliant signal characteristic MUST be advertised as a Write characteristic with write-with-acknowledgement, and write permission assigned.

The non-compliant signal characteristic MUST have a UUID of 0eb0d5f2-eae4-4a9a-8af3-a4adb02d4363.

The non-compliant signal characteristic MUST be advertised as a Write and Notify characteristic, with write permission assigned.

### Legacy service and characteristic support

It is anticipated that application may need to support two Bluetooth mechanisms whilst applications in the wild are updated
for a Herald based protocol. This section deals with those applications.

A Herald compliant app MAY advertise a legacy characteristic that supports payload read and/or write. This MAY be advertised
in the same service as the Herald protocol.

**NOTE**: The Herald API provides this support built in using the BLESensorConfiguration.legacyPayloadCharacteristic setting.

A Herald compliant app MAY advertise its own legacy service and characteristic.

**NOTE**: The app author is responsible for managing this service's state and use, not the Herald API on that device.

## Versioning

The Herald protocol uses just the above mechanisms. The Herald Project MAY introduce additional characteristics in future.
These changes MUST allow for backward compatibility and migration.

All application changes SHOULD ideally occur in the format of the data payloads and not in the underlying communication
protocol.

**NOTE**: The Herald example Payloads, for example, provides a Payload type and version 1 byte flag as the first byte
in the data payload to indicate changes to the data payload format standard being used. These standards define
themselves how versioning is applied to the data area.

## Annex A (Informative): Bibliography

Links to prior art and further reading.

- HP-1: [Bluetooth core specification](https://www.bluetooth.com/specifications/bluetooth-core-specification/) [External]
- HP-2: [Bluetooth standard assigned numbers](https://www.bluetooth.com/specifications/assigned-numbers/) [External]
- HP-3: [Bluetooth SIG homepage](https://www.bluetooth.com/about-us/) [External]
- HP-4: [Fair Efficacy Formula homepage]({{"/efficacy" | relative_url }})

## Annex B (Informative): Figures 

The below figures are also shown inline within the standard document.

None.

## Annex C (Informative): Change history

|Date|Author|Change summary|
|---|---|---|
|2020-12-09|[Adam Fowler](https://github.com/adamfowleruk/)|Existing protocol fully documented (Valid for Herald v1.1)|
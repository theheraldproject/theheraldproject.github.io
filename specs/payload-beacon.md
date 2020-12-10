---
layout: docs
title: Beacon Payload Specification
description: Beacon Payload Specification
menubar: docs_menu
---

# Beacon Payload Formal Specification

This page formally specifies Beacon Data Payload.

This specification is a **DRAFT**

## Modal verbs terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL 
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

Note that these terms apply whether they are uppercase or lowercase
and no matter what styles are applied to the phrases.

## Executive summary

The Beacon Data Payload is designed as a method to allow static venue based Bluetooth USB devices
to advertise their venue's information to nearby Herald protocol supporting applications. This
includes information about the venue and its different areas. E.g. seating areas in restaurants,
or wards and rooms in hospitals.

This allows the Herald protocol supporting phones to receive this information and build up a
virtual diary - that exists only on the phone - of when a person arrived (checked in) and left
(checked out) a venue or area within the venue.

This payload works alongside existing Herald Protocol based payloads to provide an additional
set of data useful to contact tracers and consumer device users.

This specification is meant as a replacement for manual scanning QR codes and overcomes their
limitations of ongoing incremental cost, inability to track exact check in/check out times,
and reliance on individual action to remember to scan QR codes.

It is especial important to note that the consumer device could be running anyones contact
tracing application - not just one powered by Herald - and still build up a diary of
venue visits which could later be shared with contact tracers.

## Introduction

This specification follows the standard Herald data payload international interoperability header
and provides a fixed venue identifier and venue section information locally to nearby devices.

A virtual diary can be built with a Herald powered application. A user could choose to simply
email a summary of this diary for selected days to their local contact tracing team, facilitating
rapid manual tracing and spreader event analysis.

## Scope

What is in scope

- Beacon payload that provides basic venue information

What is out of scope

- Extension data such as that to allow more accurate distance estimation and triangulation of
location visited within a venue. See the Extended Payload
specification for this.
- Contact tracing system upload and health service integration

## References

### Normative references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are necessary for the application of the present document.

None

### Information references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are not necessary for the application of the present document but they assist the user with regard to a particular subject area.

Reference implementations will be made available of this payload for iOS (swift), Android (Java) and embedded (C++). See BP-1, BP-2 and BP-3 for links.

## Definition of terms, symbols and abbreviations

### Terms

Beacon - A fixed device in a location that provides location-specific information. May be called a Bluetooth Beacon.

Big Endian - Also known as 'Network Order'. A way of encoding numeric values for transport over data networks. Bluetooth (and most network protocols) are Big Endian.

Herald Project - The opensource contributors and maintainers of the Herald website, code, and standards documents.

### Symbols

None

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

## Design Constraints

The below are the constraints placed on the design of the system that the rest of this standard is defined within.

MUST provide a read payload sufficient to provide local context information without an Internet connection

MAY provide a link to gain further information about a venue.

MUST allow a local government authority or registration company to provide an identifier that links to further venue information, if required.

MUST NOT rely on ECIES encryption or other API that are not present on iOS 9.3 or Android 5.0 or later.

MUST support Bluetooth Low Energy 4.0 or above.

MUST be able to be implemented on low cost embedded hardware that can be mass produced.

## Lifecycle Summary

A beacon device MUST conform to the Herald Protocol and its lifecycle of connections.

## Beacon Payload Data Format

A Herald Beacon MUST provide the payload data format laid out in this document.

A Herald Beacon MUST provide a Country Code and State Code for the authority providing a registry of venues. (MAY be a public or private organisation. Not limited to a healthcare authority.)

A Herald Beacon MUST provide a Location Code that is unique within the specified Country and State authority for the given venue.

A Venue MAY have one or more Herald Beacons. Each Beacon at the same venue SHOULD have a different textual description for the area in which it is located in its extended data area.

A Venue code MAY have a 'further information' URL embedded within its Extended Data area.

Below is a description of this format:-

**Figure 1. Beacon Payload Data Format**
@startmermaid
graph LR
  A(Protocol<br>Version<br>1 byte<br>0x30-0x37<br>_) --> B(Country<br> <br>2 bytes<br>uint16<br>_) --> C(State<br> <br>2 bytes<br>uint16<br>_) --> D(Location<br>Code<br>4 bytes<br>uint32<br>_) --> E(Extension<br>Data<br>0-501 bytes<br>binary<br>_)
@endmermaid

A full example for a single Beacon payload is shown below:-

**Figure 2. Example Beacon Payload**

@startmermaid
graph LR
  A(Protocol<br>Version<br>1 byte<br>0x30<br>_) --> B(Country<br> <br>2 bytes<br>826<br>_) --> C(State<br> <br>2 bytes<br>1<br>_) --> D(Location<br>Code<br>4 bytes<br>123456<br>_) --> E(Extension<br>Data<br>0-501 bytes<br>As below<br>_)
  F(Code<br>0x10<br>Premises Name<br>_)-->G(Length<br>19<br>_)-->H(Data<br>Joe's Gourmet Pizza<br>_)
  I(Code<br>0x11<br>Area Name<br>_)-->J(Length<br>14<br>_)-->K(Data<br>Window Seating<br>_)
  L(Code<br>0x13<br>Details URL<br>_)-->M(Length<br>32<br>_)-->N(Data<br>https://someurl.com/venue/123456<br>_)
@endmermaid

**NOTE**: that the [Extended Data format]({{"/specs/payload-extended" | relative_url }}) is the same as used by the Secured Payload and Simple Payload. This allows the following
data items that are of interest for a check in app:-

|Code|Description|Size and format|
|---|---|---|
|0x10|Textual short name description of premises (E.g. College, Restaurant) |1+ bytes UTF-8 String|
|0x11|Textual short name description of beacon location within premises (E.g. Seating area B)|1+ bytes UTF-8 String|
|0x12|Textual disambiguation (optional) for beacon area (E.g. Seating Area B - West wall, and Seating area B - East wall). Useful for large rooms such as sports halls, football stadiums, shopping centres/malls.|1+byte UTF-8 String|
|0x13|Location information URL (optional). MUST be https.|1+ byte UTF-8 URL|

The above list of useful Extended Data is NOT exhaustive or authoritative. Please see the Extended Data specification for full details.

## Diary Application recommendations

A Diary application SHOULD have a minimum time or signal strength (RSSI) threshold so as not to log locations a mobile phone user is just walking past in the street.

A Diary application SHOULD automatically log locations without the need for user interaction.

A Diary application SHOULD allow the user to remove check-in entries.

A Diary application SHOULD show one entry per venue on a given day even if it has data for multiple beacons at that location.

A Diary application SHOULD allow a user interface mechanism to email or export a section of the diary for sharing with contact tracers. (E.g. as a simple CSV file)

## Sharing of visit data

It is anticipated that app creators MAY NOT routinely upload all diarised contact events but MAY instead ask for them to be submitted if
a person becomes unwell.

## Exposure notification

An app SHOULD share exposure information for a venue with an exact date and start and end times for a known case being within contact with a specified venue.

An app SHOULD provide precise information regarding which beacons within the venue are relevant for an exposure during which subset of time periods for the above event.

An app MAY provide distance triangulation data specifying the location and time of specific 'hot spots' where an ill person was seated, or where people are most likely to
have been sat in order to be exposed, from the beacones within that venue at specific times.

## Reference implementations

See ConcreteBeaconPayload.java in the com.vmware.herald.sensor.payload.beacon package for the Android reference implementation. See BP-1 below for a link.

See ConcreteBeaconPayload.swift in the herald/sensor/payload/beacon folder for the iOS reference implementation. See BP-2 below for a link.

A C++ reference implementation is pending. See BP-3 below for a link.

## Annex A (Informative): Bibliography

Links to prior art and further reading

- BP-1: Android reference implementation - Coming soon
- BP-2: iOS reference implementation - Coming soon
- BP-3: C++ reference implamanetation - Coming soon

## Annex B (Informative): Figures 

The below figures are also shown inline within this standard document.

**Figure 1. Beacon Payload Data Format**
@startmermaid
graph LR
  A(Protocol<br>Version<br>1 byte<br>0x30-0x37<br>_) --> B(Country<br> <br>2 bytes<br>uint16<br>_) --> C(State<br> <br>2 bytes<br>uint16<br>_) --> D(Location<br>Code<br>4 bytes<br>uint32<br>_) --> E(Extension<br>Data<br>0-501 bytes<br>binary<br>_)
@endmermaid

**Figure 2. Example Beacon Payload**

@startmermaid
graph LR
  A(Protocol<br>Version<br>1 byte<br>0x30<br>_) --> B(Country<br> <br>2 bytes<br>826<br>_) --> C(State<br> <br>2 bytes<br>1<br>_) --> D(Location<br>Code<br>4 bytes<br>123456<br>_) --> E(Extension<br>Data<br>0-501 bytes<br>As below<br>_)
  F(Code<br>0x10<br>Premises Name<br>_)-->G(Length<br>19<br>_)-->H(Data<br>Joe's Gourmet Pizza<br>_)
  I(Code<br>0x11<br>Area Name<br>_)-->J(Length<br>14<br>_)-->K(Data<br>Window Seating<br>_)
  L(Code<br>0x13<br>Details URL<br>_)-->M(Length<br>32<br>_)-->N(Data<br>https://someurl.com/venue/123456<br>_)
@endmermaid

## Annex C (Informative): Change history

|Date|Author|Change summary|
|---|---|---|
|2020-12-09|[Adam Fowler](https://github.com/adamfowleruk/)|New payload fully documented (Ready for Herald v1.2)|

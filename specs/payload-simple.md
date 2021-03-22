---
layout: docs
title: Simple Payload Specification
description: Simple Payload Specification
menubar: reference_menu
---

# Simple Payload Formal Specification

This page formally specifies the [Simple Payload]({{"/payload/simple" | relative_url }}).

This specification is a **DRAFT** with a sample reference implementation available in Herald v1.1 for iOS and Android.

## Modal verbs terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL 
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

Note that these terms apply whether they are uppercase or lowercase
and no matter what styles are applied to the phrases.

## Executive summary

The Simple Data Payload is designed as a basic, minimum information transfer, decentralised
contact tracing protocol akin to that specified by MIT's PACT group.

This data payload consists of a rotating ephemeral key which is calculated from a daily
key. A contact tracing app can choose to upload their daily keys should the phone
owner become ill (or any other medical state). 

This daily key can then be used to calculate ephemeral keys that each device may have
seen. This allows other devices to calculate an estimation of their risk of contracting
an infectious disease, and alerting the user so they can take appropriate action.

This payload does not allow the central health service using the payload to perform
any epidemiological analysis of data submitted by apps, graph analysis of transmissions,
or detection of asymptomatic carriers or super spreaders. Please see the Secured Payload
for a standard that attempts to meet both privacy and epidemiological needs.

## Introduction

This specification follows the standard Herald data payload international interoperability header
and provides a rotating ephemeral ID which changes as the Bluetooth MAC address changes.

This prevents either the state providing the app or a casual observer scanning Bluetooth (such as
an electronic advertisement board) from tracking the owner of the phone and their movements and
associations.

## Scope

What is in scope

- Simple payload that provides basic support for decentralised contact tracing

What is out of scope

- Extension data such as that to allow more accurate distance estimation. See the Extended Payload
specification for this.

## References

### Normative references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are necessary for the application of the present document.

MIT PACT Specification - see SP-5 below for a link.

Luca Ferretti, Chris Wymant, Michelle Kendall, Lele Zhao, Anel Nurtay, Lucie Abeler-Dörner, Michael Parker, David Bonsall, Christophe Fraser. Quantifying SARS-CoV-2 transmission suggests epidemic control with digital contact tracing. Ferretti et al., Science 368, 619 (2020) (As per SP-8 below)

### Information references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are not necessary for the application of the present document but they assist the user with regard to a particular subject area.

None

## Definition of terms, symbols and abbreviations

### Terms

**Beacon** - A fixed device in a location that provides location-specific information. May be called a Bluetooth Beacon.

**Big Endian** - Also known as 'Network Order'. A way of encoding numeric values for transport over data networks. Bluetooth (and most network protocols) are Big Endian.

**Herald Project** - The opensource contributors and maintainers of the Herald website, code, and standards documents.

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

MUST provide options for a Write payload and a Read payload in order to support devices that do not provide Bluetooth Low Energy service advertising.

MUST NOT rely on ECIES encryption or other API that are not present on iOS 9.3 or Android 5.0 or later.

## Lifecycle Summary

Once a device that supports the Herald Protocol (See SP-1 below) has been identified then newly introduced devices will
need to exchange identity information. In this data payload the identity is provided by the 'Ephemeral ID'. This
is sometimes referred to as an 'exposure token'.

The device that discovers a new device MUST connect using the Herald Protocol and Read the mewly discovered device's
Data Payload. This format is described elsewhere in this standard.

The ephemeral ID (Contact ID) MUST rotate at the same frequency as the BLe MAC address. The ephemeral ID SHOULD rotate 
at the exact same time as the BLe MAC Address. This is mandatory for OS' that provide a callback to indicate this to the Herald based app.

## Simple Payload data format

Note all values are Big Endian (Network Order). All data types are C++17 data types.

**Figure 1. Simple Payload Data Format**
@startmermaid
graph LR
  A(Herald Payload<br>and Version<br>1 byte<br>0x10-0x17<br>_) --> B(Country<br>Code<br>2 bytes<br>uint16<br>_)
  B-->C(State<br>Code<br>2 bytes<br>uint16<br>_)
  C-->D(Contact<br>ID<br>16 bytes<br>binary<br>_)
  D-->E(Extended<br>Data<br>0-489 bytes<br>optional<br>_)
@endmermaid

The Country Code is the ISO 3166 Numeric (NOT textual) country code. E.g. the UK is 826. State code is assigned by the Country health authority.
See the international interoperability standard (SP-4) for details.

**NOTE**: There MUST be only one instance of the above per payload. An example is provided below:-

**Figure 2. An example Simple Payload**
@startmermaid
graph LR
  A(Herald Payload<br>and Version<br>1 byte<br>0x10<br>_) --> B(Country<br>Code<br>2 bytes<br>826<br>_)
  B-->C(State<br>Code<br>2 bytes<br>1<br>_)
  C-->D(Contact<br>ID<br>16 bytes<br>d1e931...<br>hex<br>_)
  D-->E(Extended<br>Data<br>0 bytes<br>empty<br>_)
@endmermaid

A valid Simple Payload MUST contain the Herald Protocol and version identifier followed by country code then state code then
Ephemeral ID (aka Contact ID), with values specified as Big Endian (network order) where applicable.

The Contact ID MUST be encoded as a big endian raw binary and not a hex UTF-8 string.

A valid Simple Payload MAY contain Extended Data Payload data after the Contact ID. See its specification for details. (See SP-9 below for a link)

## Key generation

The below details the cryptographic processes use to generate Daily Keys and Ephemeral IDs (aka Contact IDs).

See SP-6 and SP-7 for reference implementations.

### Cryptographic seed

The device MUST use its local cryptographic functions to generate a random seed value. 

This seed value MUST be kept on the device, and is only ever known to the device.

The length of the cryptographic seed MUST be at least 2048 bits.

**NOTE**: In the code this is called the 'secret key'.

**NOTE**: The reference implementation for the Simple Payload uses SHA1PRNG according to NIST SP800-90A
to ensure both security (with this specific use of SHA1PRNG in the NIST standard) and support in software
going back to iOS 9.3, Android 5, and is able to be calculated on low powered wearables and embedded devices
such as bluetooth fixed beacons. See SP-10 for a link to the NIST standard.

### Daily key seed

A daily key seed is generated from a reverse hash chain with truncation from the cryptographic seed.

A set of 2000 daily key seeds MUST be generated from the cryptographic seed. 

The 2000th daily key seed MUST be the SHA-256 hash of the cryptographic seed.

The 1999th daily key seed is the SHA-256 hash of the highest order 128 bits of the 2000th daily key seed. 

This MUST be repeated for each daily seed (1998th uses the 1999th data, and so on).

**NOTE**: Hashing the next days truncated data prevents reverse engineering

**NOTE**: In the code this is called a 'matching key seed'.

### Daily keys

The daily key is the key distributed by the health service for matching on phones. This means it MUST
be separated from the seeds which MUST NOT be known by the health service.

The Daily key for day i MUST be the SHA-256 hash of the XOR of the matching key seed for days i and i-1.

**NOTE**: The daily key for day 0 uses a seed for day 0 and day -1. The seed for day -1 is not stored.

### Ephemeral key / Contact ID

An ephemeral key, or generically in Herald a 'Contact ID', is the 'identity' used by the Herald protocol
with an app configured for the Simple Payload. This key typically rotates every 15 minutes along with
MAC address. In the reference implementation this could be every 6 minutes (240 IDs are generated per day).

The ephemerical key is generated using the same forward chaining mechanism for an ephemeral seed as the
daily key seed (SHA-256 with truncation), then using the same SHA-256 and XOR mechanism as the daily keys.

This approach allows a Daily Key to be exchanged on its own whereby devices can generate their view
of the ephemeral keys for matching with no knowledge of any daily key seed information.

Truncation is again used to prevent a device intercepting Bluetooth to predict what the next value is,
and thus help prevent tracking of an individuals' phone in the wild.

**NOTE**: In the Herald API this is called the 'contact key'.

Formally this is specified as below:-

A Ephemeral key seed is generated from a reverse hash chain with truncation from the daily key.

As a minimum a set of 240 daily key seeds MUST be generated from the Ephemeral seed. 

The 240th period epehemeral key seed MUST be the SHA-256 hash of the Ephemeral seed.

The 239th Ephemeral key seed is the SHA-256 hash of the highest order 128 bits of the 240th Ephemeral key seed. 

This MUST be repeated for each Ephemeral seed (238th uses the 239th data, and so on).

The Ephemeral key for period i MUST be the SHA-256 hash of the XOR of the Ephemeral key seed for periods i and i-1.

**NOTE**: The Ephemeral key for period 0 uses a Ephemeral seed for period 0 and period -1. The Ephemeralseed for period -1 is not stored.

## Matching

When a phone user becomes ill the app MUST submit their Daily Keys for the appropriate epidemiological period to the health service systems.

The health service system MUST promptly place these keys for download by health service apps and the day for which those keys are valid.

The health service MAY have more options than just 'confirmed case of COVID-19' as the status for the daily key (E.g. 'symptomatic, awaiting formal test').

The Herald Simple Payload based app MUST promptly download these keys and perform matching, and alert the user with the latest health service
advice if an epidemiological risk threshold is reached.

The total time for upload, make available for download, download, matching, and user notification MUST be less than 4 hours. See SP-8 below for rationale.

The Herald Simple Payload based app downloads these daily keys and generated ephemeral keys (Contact IDs) from this information and matches for
the days in question.

## CIA analysis summary

This is a CIA analysis of this payload. This is an informative and not normative part of this specification document.

Confidentiality - No. The Contact ID can be intercepted in the clear by any Bluetooth 
device, allowing relay and replay attacks. Only the phone who generated the Contact ID
will know who it belongs to. By rotating the Contact ID regularly (E.g. every 15 minutes)
localised Bluetooth tracking can be reduced (E.g. adboard)

Integrity - Yes. Only the sequence of codes for a particular phone can be known
by that phone for that time point as only that phone has the daily key seeds. 
Data could not be manipulated or predicted such
that an individual phone could be targeted.

Availability - Maybe. Could be compromised by a brute for attack DDoS-ing the healthcare 
system using a fake network of pre-registered devices. Amplification attacks are 
possible in this approach. This can be somewhat mitigated by using a max number 
of notified contacts per ill person submission approach, but this doesn't prevent 
the creation of a fake network with, say, 5 notifications each.

Non-repudiation - No. Neither the health authority nor receiving device are authenticated
in this approach. The healthcare system and the transmitting phone also cannot verify
that the person presenting the Contact ID is the one for whom it was initially transmitted
to.

## Reference implementations

See K.java and F.java in the com.vmware.herald.sensor.payload.simple package for the Android reference implementation. See SP-6 below for a link.

See SimplePayloadDataSupplier.swift in the herald/sensor/payload/simple folder for the iOS reference implementation. See SP-7 below for a link.

A C++ reference implementation is pending.

## Annex A (Informative): Bibliography

Links to prior art and further reading

- SP-1: [Herald Protocol Formal Specification]({{"/specs/protocol" | relative_url }})
- SP-2: [C++17 standard data types](https://en.cppreference.com/w/cpp/language/types) [External]
- SP-3: [ISO-3166 Numeric country codes](https://en.wikipedia.org/wiki/ISO_3166-1_numeric) [External]
- SP-4: [Herald International Interoperability standard]({{"/specs/payload-interop" | relative_url }})
- SP-5: [MIT PACT protocol description](https://pact.mit.edu/technical-reports/) [External]
- SP-6: [Herald Android API implementation](https://github.com/vmware/herald-for-android/tree/develop/herald/src/main/java/com/vmware/herald/sensor/payload/simple) [External]
- SP-7: [Herald iOS API implementation](https://github.com/vmware/herald-for-ios/tree/develop/herald/herald/Sensor/Payload/Simple) [External]
- SP-8: Luca Ferretti, Chris Wymant, Michelle Kendall, Lele Zhao, Anel Nurtay, Lucie Abeler-Dörner, Michael Parker, David Bonsall, Christophe Fraser. Quantifying SARS-CoV-2 transmission suggests epidemic control with digital contact tracing. Ferretti et al., Science 368, 619 (2020)
- SP-9: [Herald Extended Data Formal Specification]({{"/specs/payload-extended" | relative_url }})
- SP-10: [NIST SP800-90A defining use of SHA1PRNG](https://csrc.nist.gov/publications/detail/sp/800-90a/rev-1/final) [External]

## Annex B (Informative): Figures 

The below figures are also shown inline within this standard document.

**Figure 1. Simple Payload Data Format**
@startmermaid
graph LR
  A(Herald Payload<br>and Version<br>1 byte<br>0x10-0x17<br>_) --> B(Country<br>Code<br>2 bytes<br>uint16<br>_)
  B-->C(State<br>Code<br>2 bytes<br>uint16<br>_)
  C-->D(Contact<br>ID<br>16 bytes<br>binary<br>_)
  D-->E(Extended<br>Data<br>0-489 bytes<br>optional<br>_)
@endmermaid

The Country Code is the ISO 3166 Numeric (NOT textual) country code. E.g. the UK is 826. State code is assigned by the Country health authority.
See the international interoperability standard (SP-4) for details.

**NOTE**: there is only one instance of the above. An example may be.

**Figure 2. An example Simple Payload**
@startmermaid
graph LR
  A(Herald Payload<br>and Version<br>1 byte<br>0x10<br>_) --> B(Country<br>Code<br>2 bytes<br>826<br>_)
  B-->C(State<br>Code<br>2 bytes<br>1<br>_)
  C-->D(Contact<br>ID<br>16 bytes<br>d1e931...<br>hex<br>_)
  D-->E(Extended<br>Data<br>0 bytes<br>empty<br>_)
@endmermaid

## Annex C (Informative): Change history

|Date|Author|Change summary|
|---|---|---|
|2020-12-09|[Adam Fowler](https://github.com/adamfowleruk/)|Existing payload fully documented (Valid for Herald v1.1)|

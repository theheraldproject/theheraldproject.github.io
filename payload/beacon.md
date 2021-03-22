---
layout: docs
title: DRAFT beacon payload
description: A DRAFT payload specification for fixed beacons in restaurants
menubar: reference_menu
---

# DRAFT Beacon payload

The intention of this payload is to allow fixed beacons in shops and restaurants
that the mobile phone can use to build up a diary of movements.

This payload provides 'check in' information about the current
venue. E.g. "John's Italian, Sheffield Road, Chesterfield - Seating section A - website url"

Click here to [View the Formal Specification for the Beacon Payload]({{"/specs/payload-beacon" | relative_url }})

Instead of signing in and filling out a contact tracing form (which many
people do not bother to fill in) or require the scanning of a QR code at
a venue (which many people do not do), this history would be built up
automatically on each user's phone if they had been present under the
same beacon for a while.

The Herald envelope payload ID range 0x30-0x37 is reserved for this Payload.

This payload is identical to the Simple and Secured payloads
except that it provides a different payload format.

This approach also aids manual contact tracing for locations where people have not been
using an app.

This payload greatly reduces the workload on a venue and individual to manually 'check in'
to places they visit, increasing contact tracing ease and utility, and greatly
reducing manual contact tracing time.

## Payload content

The payload contains the following fields. Note order is Big Endian (Network order). All types
are C++ standard data types.

@startmermaid
graph LR
  A(Protocol<br>Version<br>1 byte<br>0x30-0x37<br>_) --> B(Country<br> <br>2 bytes<br>uint16<br>_) --> C(State<br> <br>2 bytes<br>uint16<br>_) --> D(Location<br>Code<br>4 bytes<br>uint32<br>_) --> E(Extension<br>Data<br>0-501 bytes<br>binary<br>_)
@endmermaid

Note that the [Extended Data format]({{"/specs/payload-extended" | relative_url }}) is the same as used by the Secured Payload. This allows the following
data items that are of interest for a check in app:-

|Code|Description|Size and format|
|---|---|---|
|0x10|Textual short name description of premises (E.g. College, Restaurant) |1+ bytes UTF-8 String|
|0x11|Textual short name description of beacon location within premises (E.g. Seating area B)|1+ bytes UTF-8 String|
|0x12|Textual disambiguation (optional) for beacon area (E.g. Seating Area B - West wall, and Seating area B - East wall). Useful for large rooms such as sports halls, football stadiums, shopping centres/malls.|1+byte UTF-8 String|
|0x13|Location information URL (optional). MUST be https.|1+ byte UTF-8 URL|

## Payload distance estimation

It is likely that the same consumer device running Herald will receive multiple beacon payloads for the same
location code, but different beacon locations within premises. 

Check-In apps supporting Herald MUST NOT provide multiple popups / notifications / registrations for these but
treat them as the same premises if the mandatory Location Code field is the same value. This reduces the
workload on the app user, and minimises the number of registrations required for multiple beacons
on one premises.

As the user moves within a location their device (phone or wearable) will take distance estimations to this
Herald beacon. A map can be created of the location either via manual creation (more accurate, more difficult), or via distance estimations
between devices with known performance characteristics in free space. (Less accurate, easier to do).

## Privacy and Security

It should be noted that the beacon will record all nearby payloads sent to it by devices. If those devices
use the [Secured Payload]({{"/payload/secured" | relative_url }}) then the 'identity' of the visitors is not known and so cannot be used as
a tracking capability by the owner of the premises or the state apparatus.

The Beacon's payload is not encrypted. This is because the venue will presumably have a sign outside the door
with the name of the premises anyway, and because this payload is intended as a public 'check-in' protocol, 
where phones are not known to the owner of the premises a priori.

This is useful for public venues and venues that allow external visitors - their Herald based apps
will be able to interact with this payload easily with no code changes between venue owners, or
complex registration requirements.


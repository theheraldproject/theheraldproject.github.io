---
layout: docs
title: Payload Extended Data Specification
description: Payload Extended Data Specification
menubar: docs_menu
---

# Payload Extended Data Specification

This page formally specifies the Extended Data Payload. This is referenced in both the
[Simple Payload Specification]({{"/specs/payload-simple" | relative_url }}) and the
[Secured Payload Specification]({{"/specs/payload-secured" | relative_url }}) and the
[Beacon Payload Specification]({{"/payload/payload-beacon" | relative_url }}).

This specification is a **DRAFT**

## Modal verbs terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL 
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

Note that these terms apply whether they are uppercase or lowercase
and no matter what styles are applied to the phrases.

## Executive summary

## Introduction

## Scope

What is in scope

What is out of scope

## References

### Normative references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are necessary for the application of the present document.

### Information references

References are either specific (identified by date of publication and/or edition number or version number) or non specific. For specific references,only the cited version applies. For non-specific references, the latest version of the referenced document (including any amendments) applies.

The following referenced documents are not necessary for the application of the present document but they assist the user with regard to a particular subject area.

## Definition of terms, symbols and abbreviations

### Terms

**Beacon** - A fixed device in a location that provides location-specific information. May be called a Bluetooth Beacon.

**Big Endian** - Also known as 'Network Order'. A way of encoding numeric values for transport over data networks. Bluetooth (and most network protocols) are Big Endian.

**Herald Project** - The opensource contributors and maintainers of the Herald website, code, and standards documents.

### Symbols

### Abbreviations

## Design Constraints

The below are the constraints placed on the design of the system that the rest of this standard is defined within.



## Lifecycle Summary








## Packet data format

Note all values are Big Endian (Network Order) and optional by default. 

@startmermaid
graph LR
  A(Data Code<br>1 byte<br>0x00-0x3F<br>Reserved<br>_) --> B(Data Length<br> <br>1 byte<br>uint8<br>_)
  B-->C(Data<br><br>1+ bytes<br>binary<br>_)
@endmermaid

Note there may be multiple sections. Below is an example showing three extended data sections:-

@startmermaid
graph LR
  A(Data Code<br>1 byte<br>0x01<br>RSSI desc<br>_) --> B(Data Length<br>1 byte<br>1<br> <br>_)
  B --> C(  Data  <br>1 byte <br>0x00<br>raw<br>_)
  C -.-> D(Data Code<br>1 byte<br>0x02<br>RSSI error<br>_) --> E(Data Length<br>1 byte<br>1<br> <br>_)
  E --> F(  Data  <br>1 byte <br>2<br> <br>_)
  F -.-> G(Data Code<br>1 byte<br>0x05<br>Approx Geo<br>_) --> H(Data Length<br>1 byte<br>3<br> <br>_)
  H --> I(  Data  <br>4 bytes<br>S41<br> <br>_)
@endmermaid


## Data values supported

Any data item with no value should not be included in the data payload. 
I.e. 0 byte length extended segments are omitted.

|Code|Description|Format and size|Example|
|---|---|---|---|
|0x00|Extension data area format version ID (assume “1” if not present).|uint8|
|0x01|RSSI description.|1 byte hex| 0x00 = raw<br>0x01 = sample mean<br>0x02 = running mean<br>0x03 = sample mode<br>0x04 = running mode|
|0x02|RSSI error bound - Expressed as +/- RSSI.|uint8|3|
|0x03|Sonar range estimate - decimal metres (size = type of float, use C++ standard types).|Float. E.g. float-32 or float-64|6.54|
|0x04|Sonar error bound - Expressed as +/- metres, single value. 4+ bytes, but float. E.g. float-32 or float-64. 0x03 must be present.|Same as 0x03|1.08|
|0x05|Approximate geo location (textual) - E.g. in UK “Postal District” which is ~6000 homes.|1+ bytes UTF-8 |'S41' or '53800'|
|0x06|Approximate geo co-ordinates - EPSG:4826 (WGS 84) - minutes (0.0166 degrees), accurate to ~1852 metres|Two signed int16. Lat then Lon|
|0x07|As 0x06, but EPSG:3857 (Web mercator projection)|As 0x06||
|0x08|As 0x06, but EPSG:7789 (ITRF2014)|As 0x06||
|0x09<br>-<br>0x0F|Reserved for future geo data sections|1+ bytes||
|0x10|Textual short name description of location|1+ bytes UTF-8 String|Joe's Pizza|
|0x11|Textual short name description of beacon location within premises|1+ bytes UTF-8 String|Seating Area B|
|0x12|Textual disambiguation (optional) for beacon area. Useful for large rooms such as sports halls, football stadiums, shopping centres/malls.|1+byte UTF-8 String|East Wall or West Wall|
|0x13|Location information URL (optional). MUST be https.|1+ byte UTF-8 URL|https://www.someurl.com/myvenue|
|0x14|Full Address|1+ byte UTF-8. \n as newline|121 High Street\nScunthorpe|
|0x15|Entry Source|1+ byte UTF-8|"ble" for Bluetooth Low Energy (i.e. Beacon)|
|0x16|Entry Source Version|1+ byte UTF-8|N/A for Herald beacons (See QR codes in Beacon standard)|
|0x17|Location company ID (For register in 'Country' and 'State')|1+ byte UTF-8 or numeric, as per country/state standard|Variable|
|0x18|Location company ID (GS1 only)|uint32|9012345|
|0x19|Reserved for future Herald standard use|
|0x20|Contact event start time|uint32 seconds since Unix Epoch. May be 'present time' - useful for transmitting time to wearable devices with no external time reference|1608062077 (Tue 15 Dec 2020 ~19:54)|
|0x21|(Contact) Event end time|uint32 unix epoch seconds|Same example as above|
|0x22<br>-<br>-0x3D|Reserved for future Herald standard use|
|0x3E|Indicates the following data is encrypted for decryption by the originating phone|1+ bytes, binary|
|0x3F|Indicates the following data is encrypted for decryption by the originating phone’s health service/authority|1+ bytes, binary|
|0x40<br>-<br>0x7F|Reserved for future Herald standard use|
|0x80<br>-<br>0xFF|Custom country / application use|




## Annex A (Informative): Bibliography

Links to prior art and further reading

## Annex B (Informative): Figures 

The below figures are also shown inline within the standard document.



## Annex C (Informative): Change history

|Date|Author|Change summary|
|---|---|---|
|2020-12-15|[Adam Fowler](https://github.com/adamfowleruk/)|Added additional entries from Beacon standard|
|2020-12-09|[Adam Fowler](https://github.com/adamfowleruk/)|Existing payload fully documented (Valid for Herald v1.1)|
|2020-12-05|[Adam Fowler](https://github.com/adamfowleruk/)|Initial draft content|



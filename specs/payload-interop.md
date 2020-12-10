---
layout: docs
title: International Interoperability Specification
description: International Interoperability Specification
menubar: docs_menu
---

# International Interoperability Specification

This page formally specifies the [International Interoperability]({{"/payload/interop" | relative_url }})
protocol and data payloads.

This specification is a **DRAFT**


## Contents

**NOTE:** No contents or formal notation as of yet. Placeholder content until formal spec format decided.

## Figures

None

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

@startmermaid
sequenceDiagram 
  Phone A->>Health System A: Upload contact events
  Health System A->>Phone A: Upload successful
  Health System A->>Health System B: Forward contacts
  Health System B->>Health System A: Forwarding successful
  Health System B->>Health System B: Validate exposure service tokens
  Health System B->>Health System B: Calculate risk subtotals
  Health System B->>Health System B: Update exposure list
  Phone B->>Health System B: Request exposure list changes
  Health System B->>Phone B: Respond with list changes
  Phone B->>Phone B: Validate exposure confirmation tokens
  Phone B->>Phone B: Increment exposure, check against threshold
  Phone B-->>Phone B: If exceeded, alert user
@endmermaid







## Annex A (Informative): Bibliography

Links to prior art and further reading

## Annex B (Informative): Figures 

The below figures are also shown inline within the standard document.


Figure 1. International Interoperability flow diagram

![International Interoperability stages](../images/SecuredPayloadInternationalInterop.png)

Figure 2. Contact event upload data format (optional)

TODO

Figure 3. Contact event sharing data format (required)

TODO


## Annex C (Informative): Change history

|Date|Author|Change summary|
|---|---|---|
|2020-12-09|[Adam Fowler](https://github.com/adamfowleruk/)|Existing payload fully documented (Valid for Herald v1.1)|
|2020-12-04|[Adam Fowler](https://github.com/adamfowleruk/)|Initial draft content|
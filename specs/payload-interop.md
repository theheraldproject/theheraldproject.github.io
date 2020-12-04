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

## Changes and errata

The below is the version history:-

|Date|Author|Change summary|
|---|---|---|
|2020-12-04|[Adam Fowler](https://github.com/adamfowleruk/)|Initial very draft content|

## Contents

**NOTE:** No contents or formal notation as of yet. Placeholder content until formal spec format decided.

## Figures

Figure 1. International Interoperability flow diagram

![International Interoperability stages](../images/SecuredPayloadInternationalInterop.png)

Figure 2. Contact event upload data format (optional)

TODO

Figure 3. Contact event sharing data format (required)

TODO

## Interaction flow diagram

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


---
layout: docs
title: Disease Control
description: Herald's end to end disease control backend
menubar: about_menu
---

# Disease Control

In addition to mobile applications and wearables there is a need for central healthcare systems
to support these applications. This includes device enrolment and exposure token upload/download
to support Digital Contact Tracing (DCT) mobile apps and wearables, but also other backend
modules necessary for disease control.

Herald will soon launch and end to end system for Digitally Assisted Contact Tracing (DACT).
[See our blog announcement]({{"/blog/dct-still-needed" | relative_url }}) for a view of why this is necessary.

There is a need for various backend public health applications to help control disease spread.
The first of these we aim to develop are below:-

- Digital Contact Tracing (DCT) App registration, advice updating, and exposure token upload/download
- A REST API to expose contact prioritisation to existing manual contact tracing systems, helping them prioritise who to contact based on exposure risk scores

We can also see a need for future modules. Although we've not prioritised these for release yet, there is a clear need for:-

- Digital Contact Tracing (DCT) International Interoperability - privacy protecting way to exchange contact keys between different PHAs worldwide
- Full manual contact tracing web user interface - for jurisdictions without an existing application here
- Epidemic progress dashboards - to see current and past state (can be extended in future with predictive analytics, or prescriptive analytics for decision support on changing controls in place. E.g. local lockdowns)
- Spotting disease analytics - Local R number calculation, asymptomatic carrier detection, new disease / variant highlighting, geographic trend analysis

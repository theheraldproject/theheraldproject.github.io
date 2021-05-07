---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Bluetooth detailed research
description: Full background information in to the Bluetooth issues we discovered and how we worked around them.
menubar: research_menu
---

# Bluetooth Research

During our research we discovered a number of low-level Bluetooth issues on mobile phones. We details those here plus the workarounds that work or do not work.

## Hardware issues

The older the [mobile phone hardware]({{"/bluetooth/hardware" | relative_url }}) the worse its performance. 
No surprises there. There are certain devices,
or pairings of devices, that cause issues. These are detailed in this section.

## Operating System issues

There are a number of ways [Operating Systems]({{"/bluetooth/os" | relative_url }}) behave or bugs that are in how certain versions of those
Operating Systems operate. The main one hampering Bluetooth based contract tracing apps is the
'Apple device not discovering other Apple devices in the background' bug within iOS. This and
other issues are detailed in this section.

## Crowding issues

The bigger the croud, the louder the talk. No difference with radio based protocols like Bluetooth.
We detail issues we found and workaround and approaches to minimise interference and cross talk here. We generally call this issue [Crowding]({{"/bluetooth/crowding" | relative_url }}) as shorthand.

## Distance estimation calibration

Whilst not directly the subject of our research we have some insight 
in to [Distance Estimation Calibration]({{"/bluetooth/distance" | relative_url }}) and so include this information here.

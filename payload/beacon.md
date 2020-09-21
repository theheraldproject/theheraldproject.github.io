---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: DRAFT beacon payload
description: A DRAFT payload specification for fixed beacons in restaurants
menubar: docs_menu
---

# DRAFT Beacon payload

The intention of this payload is to allow fixed beacons in shops and restaurants
that the mobile phone can use to build up a diary of movements.

This payload provides 'check in' information about the current
venue. E.g. "John's Italian, Sheffield Road, Chesterfield - Seating section A - website url"

Instead of signing in and filling out a contact tracing form (which many
people do not bother to fill in) or require the scanning of a QR code at
a venue (which many people do not do), this history would be built up
automatically on each user's phone if they had been present under the
same beacon for a while.

The Squire envelope payload ID range 0x30-0x37 is reserved for this Payload.

This payload is identical to the Simple and Secured payloads
except that it provides an additional read characteristic
and payload for that read.

This approach also aids manual contact tracing for locations where people have not been
using an app.

This payload greatly reduces the workload on a venue and individual to manually 'check in'
to places they visit, increasing contact tracing ease and utility, and greatly
reducing manual contact tracing time.

## Payload content

Coming soon!...

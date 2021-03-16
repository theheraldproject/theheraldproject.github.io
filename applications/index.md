---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Applications
description: Applications of Herald
menubar: docs_menu
---

# Applications of Herald

Whilst Herald came about through work on creating a reliable Bluetooth stack
to enable Contact Tracing applications to be more useful, there are many more
applications for the Herald Protocol.

This section details the uses it can be put to, both within the healthcare
arena and in many other areas.

- Situational awareness apps (All using Bluetooth mesh too)
  - Herald can act as the 'Gateway' protocol between a Bluetooth Mesh Relay Node and consumer and work mobile devices
  - Build a virtual map of moveable equipment using watch battery powered Bluetooth tags
  - Provide a wayfaring capability to anyone in your facility. Useful for complex locations like hospitals, university campuses, and museums
  - Provide a 'where are my family members?' capability to holiday makers in a hotel or resort
- Communication apps
  - File sharing between Android and iOS devices, reliably
  - Local 'same location' peer to peer applications, such as instant messaging or gaming apps
- Healthcare
  - Contact tracing reliable protocol and distance estimation
  - Replacement network (also using Bluetooth Mesh) for phone based beepers in hospitals - provide more details to 'beeped' staff
  - Patient/resident tracking and vitals monitoring in hospitals and care homes (also using Bluetooth Mesh)
    - Wearables rather than phones are a good option here. E.g. for young children or the elderly without phones
- Safety apps
  - Using beacons in high-risk areas, an employee exposure app could accurate record exact exposure to hazardous environments
  - Also using beacons, know where to deep clean if an employee does fall ill at your large campus
  - Check in app - Walk around and be let in to secure areas automatically
  - Rescue app - E.g. for skiing/snowboarding avalanche rescue - find the hidden/non visible person. Could be fire in a large building, or rescue on a tube train

These fall in to the following separately documented categories:-

- [Point to Point / Device to Device use]({{"/applications/p2p" | relative_url }})
  - Including [Digital Contact Tracing]({{"/applications/ct" | relative_url }})
- [Bluetooth Mesh interactions]({{"/applications/mesh" | relative_url }})

Many other use cases are possible.

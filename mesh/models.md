---
layout: docs
title: Herald Bluetooth MESH Models
description: Custom Bluetooth MESH models for Herald healthcare use cases
menubar: reference_menu
---

# Healthcare Vendor Models from Herald

THe Herald Project have implemented a number of Vendor models and are evaluating
these prior to suggesting to the Bluetooth SIG that they be adopted as part
of the Bluetooth MESH standard.

The Herald Project also re-use Bluetooth SIG MESH models, servers and clients.
This means that if you have a Bluetooth SIG MESH compliant device you will be
able to use it with a Herald Bluetooth MESH Network and be able to provision,
configure, and manage it using our permissively licensed open source software.

The idea is to create a set of standard models that enable multiple vendors
to interoperate within a public health ecosystem or Smart Hospital. The
Herald Project welcomes other vendors, Public Health Authorities, or
individuals with a passion for healthcare in helping us design and use these
models.

## Herald Vendor Models

The Herald Project is a Series of Linux Foundation Public Health. This means
we use the same Bluetooth Vendor ID (0x05f1) as other Linux Foundation projects,
with the main one being the 
[Zephyr RTOS project](https://zephyrproject.org/)
which Herald uses as the base Operating System for all our wearable and
beacon applications.
All Herald Project models have IDs starting at 0x2000.

We have implemented a number of models for Bluetooth MESH:-

- Herald Venue Beacon Server (0x2001)
  - This allows a MESH adminstrator to programme the data emitted over Bluetooth Low Energy to passing Bluetooth devices such as Mobile Phones
  - This LE Beacon can be used for internal building navigation (if the Location URL field is set)
  - These LE beacons can also be used to create a venue diary with auto check-in and check-out on a mobile phone which can be shared by a user with contact tracing teams
  - This is used by the Herald MESH Relay application
- Herald Presence Server (0x2002)
  - This server publishes nearby LE device aggregated information (E.g. Mean RSSI) over Bluetooth MESH
  - This can be left as all Bluetooth LE devices, or a specific subset
    - All devices allows an anonymous analysis of footfall or occupancy density, useful for emergency evacuations or when trying to minimise social mixing in a hospital
    - Restricting to just Bluetooth MESH or Bluetooth LE Herald Tags allows the tracking of Hospital Equipment
    - The same mechanism could be used to track wristbands or wearables that were Bluetooth LE or MESH tags, useful for Dementia patients or at-risk patients
  - This is used by the Herald MESH Relay application

We are also considering implementing the following MESH models:-

- Herald MESH Tag Server (ID TBD)
  - Allows remote reprogramming of a Bluetooth MESH tag
  - Publishes regular presence information to the MESH when the device it is attached to is moving
  - Acts as a MESH low power node when the device is not moving, using a nearby MESH Relay as a 'Friend' to store presence information, and thus enter a low power sleep mode
  - Provides additional security for equipment tags over Bluetooth Low Energy
  - To be used by the Herald MESH Tag application for long term, low power, tags using a low-power Herald Wearable V2 tag

## Linux Foundation MESH models and Operation Codes

There are currently no other vendor models being used by other Linux Foundation projects.
We have defined a set of Linux Foundation Op Codes and try to ensure we keep other
Linux Foundation projects aware of these to facilitate interoperability. These are:-

- All of these are currently DRAFT op codes
- Get Model Status (0x01)
- Set Model Status (0x02) with acknowledgement
- Set Model Status (0x03) without acknowledgement
- Status Message (0x04)

Note that all of the above codes must be used with the Linux Foundation Vendor ID (0x05f1).

## Bluetooth SIG Standard Models used by Herald

The following are required for all MESH devices in the Bluetooth MESH specification and
so are present in all Herald MESH applications:-

- MESH Configuration Server
- MESH Health Server

We are also considering using the following standard models:-
- MESH Time Server on all Herald MESH Modem devices, if enabled, with time from NTP sources
- MESH Time Client on all Herald MESH Relay and Tag devices, subscribed to the MESH Modem time group
   - This can also be used to expose a Bluetooth Low Energy Time Service via GATT on devices with a Herald Venue Beacon defined

The Herald Modem also uses the heartbeat functionality of the Health Server to
publish network communication diagrams and performance information to
RabbitMQ. This allows it to be tracked over time and visualised in cloud native
applications.

The Herald Project also adopts a standard layout of MESH Elements if supported
by the device:-

- Element 1 (Primary Element) - Bluetooth SIG standard functionality
  - Configuration Server
  - Health Server
  - (Future) Time Client or Time Server
- Element 2 - Presence related services
  - OnOff server (to denote whether to enable the Presence Server)
  - Herald Presence Server - which can be subscribed to
- Element 3 - Beacon related services
  - OnOff server (to denote whether to expose a Herald Venue Beacon and GATT Time Service)
  - Venue Beacon Server (to programme the information for the Beacon)

## Future additions

We are also considering the future mechanisms:-

- File Transfer Model
  - To open direct Bluetooth Low Energy connections between MESH nodes for file transfer
  - A secure MESH way to configure the [GATT Bluetooth Object Transfer service](https://www.bluetooth.com/specifications/specs/object-transfer-service-1-0/)
  - We're interested in this to enable Over The Air (OTA) application and firmware updates to MESH connected devices
  - The MESH Modem will transfer files to a select subset of MESH Relays, then instruct the relevant MESH devices to open an LE connection and update themselves from this binary
  - This will exchange LE binding keys over Bluetooth MESH directly between the two nodes involved in the transfer, making it more secure than LE Object Transfer alone (Effectively the MESH message is an Out of Band security key exchange)
  - The secure bootloader will validate the update before applying it, preventing unsigned or tampering of updates over the air
  - MESH is not suitable for large file transfers, so this hybrid mechanism may be useful

## Herald MESH standard publish/subscribe model

Herald does not hardcode the group IDs used for publication or subscription of MESH messages.
This is to allow people to customise their messages. When a Herald MESH Modem is used to
configure a network, however, it will take a standard approach to the types of group it
creates, and which nodes are added as subscribers or publishers.

- Presence Information
  - All Herald Presence Servers are assigned a Presence application key and instructed to publish their messages to a Presence Messages Group
  - This is a different application key per Presence Server, ensuring that a compromised MESH node cannot decode all presence information
  - All Herald MESH Modems' Presence Clients subscribe to the Presence Messages Group and may pass this onto the computer they are attached to
    - Which computer receives the messages is determined by the server-side HA/DR configuration and out of scope of the MESH model
- Beacon Configuration
  - All MESH Relays with a Beacon service are assigned a Beacon application key and does not publish or subscribe their models
  - Each application key is different per beacon, ensuring that a compromised MESH node cannot maliciously set all beacon information
  - All Herald MESM Modems' Venue Beacon Clients can set and get Beacon information if they hold this particular application key


## Find out more

You can now view the [Herald MESH API](/mesh/api) page to find out how to interact with this API.
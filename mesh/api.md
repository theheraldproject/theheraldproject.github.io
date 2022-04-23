---
layout: docs
title: Herald Bluetooth MESH API
description: How to use the Herald MESH API
menubar: reference_menu
---

# Herald Bluetooth MESH API

The MESH API consists of several parts:-

- General use MESH model and operation code definition constants and identifiers
   - To be used on any app in any platform
   - See herald-for-cpp/herald/include/herald/mesh/mesh.h for these definitions
   - This header file can be used in C++ or C applications on any platform
     - TODO MAKE THIS TRUE
- Zephyr Herald MESH reference implementation API
   - A Zephyr-only set of C files, usable in a Zephyr app, for client and server MESH model interaction
   - See herald-for-cpp/herald/include/herald/mesh/zephyr.h for Zephyr MACRO definitions
   - Each service has its own C server and client implementation:-
     - SERVICENAME.h defines platform-independent constants. E.g. presence.h, location.h and modem.h
     - SERVICENAME-client.h defines platform-independent C callbacks and C++ wrappers for platform specific client implementations
     - zephyr/SERVICENAME-server.h defines a Zephyr server for this model in C
     - zephyr/SERVICENAME-client.h defines a Zephyr client for this model in C
- Herald MESH Modem commands API
   - A mix of low level (Zephyr MESH Shell command like) and higher level (Herald MESH use specific) commands
   - Sent over a CMC ACM serial device using protobufv3 messages
   - Modem-side is a Zephyr only reference implementation
     - zephyr/modem_usb.h defines the CDC ACM protobufv3 message processor. This uses service client models to implement commands.
   - App-side is a standalone C API with C++ wrapper objects
     - Default implementation is a containerisable app that links the Mesh Modem to RabbitMQ queues and exchanges
     - See modem.h and modem_client.h for multi-platform API to interact with a USB conencted Herald Bluetooth MESH Modem
- Provisioning
   - We had to implement some standard provisioning functionality
   - no_oob_prov.h - A no security provisioner. DO NOT USE IN PRODUCTION
   - (COMING SOON) nfc_prov.h - An NFC based OOB provisioner, intended for initial provisioning
   - After provisioning the GATT_PROXY is still running to allow application key setting using the nRF Mesh mobile app (or similar)
   - Once app keys are shared, and the relay is set to 'configured', the GATT_PROXY service is disabled for security
   - All provisioning / resetting / configuration is then done via the MESH itself

## Kconfig defines

(TO BE COMPLETED) The API supports Kconfig. The following defines are available:-

- CONFIG_BT_MESH_HERALD_MODEM_SRV (default: n) - Enable MESH Modem Server functionality
- CONFIG_BT_MESH_HERALD_PRESENCE_SRV (default: n) - Enable MESH Presence Server functionality
- CONFIG_BT_MESH_HERALD_LOCATION_SRV (default: n) - Enable MESH Location Server functionality (i.e. remote programming of LE Venue Beacon. Does not enable/disable LE Venue Beacon functionality)

## Herald MESH and RabbitMQ

Why have we used RabbitMQ? You could set up an entire MESH using the Nordic mRF MESH mobile application... But it would be hellish to
manage and debug, and so we thought we'd do it another way. It's also more secure without having to have the GATT RELAY enabled after
provisioning has been completed.

To enable this we created the MESH Modem that has a full Bluetooth SIG MESH set of low-level commands,
and some Herald MESH specific higher level commands which will use multiple orchestrated
low-level MESH commands to achieve their goals.

RabbitMQ is used as it's a reliable messaging system and can be used for both individual modem
command queues, but also for publishing data from the MESH, such as presence data, into
RabbitMQ to allow it to be consumed by applications.

This also allows a mechanism for interacting with Hospital clinical applications, or
appointments applications, or building control applications (for standard MESH lighting uses perhaps).

It is most likely in a large site that server-side IT controls will be required to secure and
configure a large network and all the devices within it over time. It was therefore logical
to abstract away the underlying command mechanism, facilitating a wide range of application developers,
rather than just Embedded systems developers, to manage a Bluetooth MESH network.

It should be noted that although there are likely pre-configured Herald specific message
queues, any publish/subscribe/configure function in Bluetooth MESH can be achieved by using
low level commands and binding to RabbitMQ queues. This allows third party vendors to
integrate their own Bluetooth MESH systems to a Herald MESH, or to just lift the MESH Modem
client and server code for use in their own systems.

## Where to go next?

The applications and API are still in a state of flux. When they are released for Beta in
May 2022 we'll update this page with links to the full API documentation and application
samples.

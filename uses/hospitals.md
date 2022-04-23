---
layout: docs
title: Hospital Use Cases
description: Hospital use cases for Herald
menubar: docs_menu
---

# Hospital Use Cases

By placing a low-cost set of [Herald Bluetooth MESH](/mesh) devices around a hospital several use cases can be supported:-
- Site Navigation - Allow staff, patients and visitors to navigation around your hospital using their mobile phone
- Locating equipment - Track expensive equipment such as syringe pushers, ECG machines, or track samples taken in hospitals. Many hospitals purchase more equipment than needed due to not being able to find it when required. This leads to staff hoarding equipment in side rooms and behind screens. You can now locate them using a Herald tracker attached to the device. These only need recharging once a year.

Emerging solution areas we're looking into now include:-
- Secure Messaging - Replacing expensive hospital pagers with a reliable, internal, secure, and patient context rich systems linked to their PCs, tablets and phones.
- Out-patient services - by linking the navigation functionality to patient sign-in and appointments systems, you can have patients sign in from their phones rather than approach a reception desk or sign in kiosk, and let patients know when their specialist is delayed, directing them to a cafe or for a walk in the gardens, and alerting them when they need to go to a waiting room. This reduces loitering in waiting areas (bad for COVID safety!), and is a more pleasant and customer-informed way of running a service.

## Where to get these solutions

Herald is an open source project. You are more than free to download our source code and build your own solution from it. There are also commercial companies you can get support from. Some of these are shown below.

- [VMware Tanzu Labs](https://tanzu.vmware.com/labs) - Herald was a VMware originated open source project. VMware Tanzu Labs provides 1:1 pairing and training of organisations' own development teams in best practice agile methodologies and typically pairs with customers on a first project. This leaves your organisation with both trained staff that are self sufficient and a working software product! Tanzu Labs often help with the development of Herald during 'beach time' between projects, and so have experience in using Herald. Tanzu Labs worked with Alberta Health to rearchitect the Alberta TraceTogether application on to Herald.
- [FaceDrive Health](https://health.facedrive.com/) - A contributor to Herald, this Canadian eHealth wearables and system manufacturer supplies wearables and a private company deployable Digital Contact Tracing solution. They can also supply wearables, beacons, and consultancy. FaceDrive Health are investigating the use of Herald MESH for safety in a variety of contexts.

## Open source applications for Hospitals

The following applications for Hospitals will be published soon by the Herald project and other Linux Foundation Public Health projects for your use:-

- A [Herald Bluetooth MESH API](/mesh/api) that allows you to build a variety of innovative applications:-
  - The MESH relay application provides the Bluetooth MESH backbone for all of these use cases
  - The MESH modem application links your Bluetooth MESH to your hospital servers
- A Herald C++ MESH link application between the MESH modem and a RabbitMQ reliable message bus, enabling:-
  - A Kubernetes for Healthcare MESH management web application to remotely control all devices in the MESH
  - A Kubernetes for Healthcare equipment and public safety web application to find equipment and view venue use density and footfall

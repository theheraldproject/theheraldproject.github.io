---
layout: docs
title: OpenTrace
description: Digital Contact Tracing and Exposure Notification with OpenTrace
id: opentrace
menubar: about_menu
---

# OpenTrace

OpenTrace was created by [GovTech Singapore]( [https://tech.gov.sg](https://tech.gov.sg) ) in March 2020 and was the 
first publicly available and open source Digital Contact Tracing system in the world based
on Bluetooth Low Energy. OpenTrace supported the [BlueTrace protocol]( [https://bluetrace.io](https://bluetrace.io) ).

![OpenTrace Logo](/img/opentrace.png)

GovTech Singapore [donated OpenTrace]({{"/blog/opentrace-donation" | relative_url }}) to LFPH and the Herald Project in August 2021. The original code
has been kept for posterity and will be used as a base for future developments. 
A new OpenTrace version is being worked on by the Herald Project
to provide a long term open source system to help prevent the spread of future diseases.

More details will be released soon. For now, the aims of OpenTrace v2 are:-

- Provide an end-to-end, whitelabled system that PHAs can adopt and roll out within a week without large teams to maintain
- Place the end user in control of how much, or how little, information is shared to their Public Health Authority or others
- Provide a back-end for the system that gives useful epidemiological data on disease progression, whose efficacy can be proven in epidemiological studies
- Allow the code to be treated as 'upstream', allowing PHAs to take new versions, additional modules and functionality, in future
- Be a pluggable system, allow other projects in LFPH and third parties to provide additional modules to PHAs (E.g. a manual contact tracing case management system for those countries without one currently)
- Provide whitelabeled mobile apps for Android and iOS (supporting more than just the most recent phones)
- Provide an open source wearable hardware design and application to allow distribution at low cost, worldwide (around USD 15 per wearable)
- Support multiple Bluetooth layer protocols for detection - Herald, Bluetrace, GAEN, and Singapore's TraceTogether

## Code repositories

The donated OpenTrace v1 repositories can be found below:-

- OpenTrace iOS application - [https://github.com/theheraldproject/opentrace-v1-ios](https://github.com/theheraldproject/opentrace-v1-ios)
- OpenTrace Android application - [https://github.com/theheraldproject/opentrace-v1-android](https://github.com/theheraldproject/opentrace-v1-android)
- OpenTrace cloud functions (server side) - [https://github.com/theheraldproject/opentrace-v1-cloud-functions](https://github.com/theheraldproject/opentrace-v1-cloud-functions)
- OpenTrace calibration - [https://github.com/theheraldproject/opentrace-v1-calibration](https://github.com/theheraldproject/opentrace-v1-calibration)

The new OpenTrace v2 repositories will be made available soon.

## FAQ

Below are some frequently asked questions about OpenTrace.

### What is the difference between the Herald API and OpenTrace v2?

Herald is a low-level API for end user devices to provide proximity detection,
data exchange, and risk and exposure analysis. This is useful in many applications,
from file sharing with friends on Bluetooth, to navigation inside a complex building,
and of course as part of a Digital Contact Tracing and Exposure Notification application.

OpenTrace is an end-to-end system that aims to control the spread of pandemic diseases.
This includes mobile applications that contain the [Herald API]({{"/herald" | relative_url }}), but also includes the full
range of back end components for Public Health Authorities, and mobile applications that
can be whitelabled and configured for each state or country deploying them. OpenTrace is
both a Digital Contact Tracing and Exposure Notification system.

The Herald Project is also the name for our group of open source volunteers 
working on both code bases. When we say "Herald" we mean the API, when we say 
"The Herald Project" we mean the overarching project and its contributors.

### What does Whitelabling mean?

Whitelabling is the practice of producing a single application or system with identical
components whose user interface and configuration can be altered for each organisation.
This allows Public Health Authorities to take the OpenTrace v2 code and simply configure
it, drastically reducing the time and cost of deploying and maintaining the system.

Whitelabling is a common practice in the software industry. Have you ever shopped online
for car insurance and wondered why each companies quotation website is almost identical
apart from the branding and a handful of questions? This is because many use the same
quoting software which is whitelabled, and then customised for each insurance provider.

### Why are the OpenTrace v1 and v2 repositories separate?

Partly as a practical step for maintainability, but also partly for nostalgia and to
preserve OpenTrace v1. As one of the world's first open source Digital Contact Tracing system,
OpenTrace v1 marks humanities first attempt to use end-user digital technology to
limit disease spread. It has been the basis of many countries' Digital Contact Tracing
systems. The team felt it right to preserve this first version for the future.

OpenTrace v1 and v2 are designed for different consumers too. Whilst OpenTrace v1
was published as open source so other countries could implement their own Digital
Contact Tracing systems, it was never meant to be maintained long term with
multiple countries contributing to a single code base. 

OpenTrace v2 is designed
for international collaboration on a common, community led, and openly governed
basis. Rather than take a copy of OpenTrace v2 and customise it, countries are
encouraged to take each release of OpenTrace and run it as-is, with their own
configuration and their own modules plugging into it. In this way they
can take future updates and improvements, and any code they contribute back
to OpenTrace within the Herald project will benefit all countries throughout
the world.

### How will OpenTrace v2 be different from other systems?

Our focus is on helping as many countries in the world as possible to tackle
the spread of diseases. Whilst COVID-19 has brought pandemic diseases to the 
forefront of attention throughout the world, there are many diseases whose
spread devastates the lives of many people. Ebola, SARS and TB are just some
other examples from very recent history. OpenTrace v2 will be ready for future
emerging strains and diseases.

OpenTrace v2 will be designed in a way to allow its epidemiological 
effectiveness to be observed and measured,
providing vital information to countryies' medical regulation authorities. Many
COVID-19 apps with current approval have done so on an emergency exemption basis.
It is important for ethical reasons to be able to prove the efficacy of these
systems, and OpenTrace v2 will be designed in a way to allow this to be proven.

OpenTrace v2 will be available on a wide range of devices - not just the latest
(and expensive!) smartphones from the major manufacturers. Older phones will
also be supported, as too will third party devices such as wearables. Third party
manufacturers such as smartwatch creators will also be able to integrate with
and use OpenTrace. This democratices access to this life saving technology, bringing it
into the reach of every individual in every jurisdiction for the first time.

OpenTrace v2 will aim to integrate to and work with existing health care
systems and processes, such as manual contact tracing, rather than exist as a
standalone and black-boxed Exposure Notification system for a single disease.

As an example, imagine a manual contact tracing case management system that
already exists in a country. Should users of a mobile device with an OpenTrace v2
app on it choose to share their personal details, when a contact tracer
is deciding who to call first on an ill person's recent contact list they could
potentially see the strength of exposure of that persons contacts, allowing them
to prioritise who to call, and influencing their advice to that person. For a fleeting
contact they may just let the person know to remain vigilant, whereas for a 
large exposure they may advise a person to self isolate and get tested for a disease.
This allows countries to adopt a data driven approach to isolation before having to
resort to full regional or national lockdowns, whilst keeping end users in control
of who has access to their information.

OpenTrace v2 will be implemented in a way to put the user in control of how much or
little information they share, but should they choose to share this information,
it will allow a country's Public Health Authority to provide more accurate and tailored
advice to the individual, and allow them to track the spread of disease in their country.

OpenTrace v2 will also support multi-protocol device interoperability from the outset.
Bluetooth protocols such as BlueTrace (OpenTrace v1), Singapore TraceTogether, Herald and the Google
Apple Exposure Notification (GAEN) protocols will all be supported, both at a device
level and an exposure notification level on the OpenTrace v2 backend software.
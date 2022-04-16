---
layout: docs
title: Public Health Use Cases
description: Public health use cases for Herald
menubar: docs_menu
---

# Public Health Use Cases

Herald was create originally to help create reliable and open source 
(Digital Contact Tracing)[/applications/ct]
applications for the SARS-Cov-2 (COVID-19) pandemic.

Herald's use has since expanded, but as a project within the
(Linux Foundation Public Health)[https://www.lfph.io/join/projects/]
we continue to find new ways to help in healthcare.

Herald has a number of current applications within Public Health:-
 - The [Herald API](/guide/api) is used by several Digital Contact Tracing applications, including Australia's COVIDsafe 2.0 and Alkberta, Canada's Alberta TraceTogether
 - The [OpenTrace v2](/opentrace) Digital Contact Tracing app and system is being developed within the Herald project to act as an insurance policy for the next pandemic
 - Herald [Venue Beacons](/applications/beacons) allow a phone to collect a Venue Diary of where a person has gone, allowing them to better inform Public Health Authorities when they are manually contact traced. This stores information that is compatible with the QR code systems of New Zealand and the UK, allowing for wide interoperability but in an automatic and more user friendly way than expecting manual QR code scanning.
 - Tracking your social mixing score allowing you to make an informed decision about controlling your social mixing in a pandemic, and doing this on-device in a privacy preserving way. We're able to do this accurate thanks to the Herald innovation of mobile phone Bluetooth self-calibration, which makes accurate risk scoring possible on mobile devices.

## Where to get these solutions

Herald is an open source project. You are more than free to download our source code and build your own solution from it. There are also commercial companies you can get support from. Some of these are shown below.

- [VMware Tanzu Labs](https://tanzu.vmware.com/labs) - Herald was a VMware originated open source project. VMware Tanzu Labs provides 1:1 pairing and training of organisations' own development teams in best practice agile methodologies and typically pairs with customers on a first project. This leaves your organisation with both trained staff that are self sufficient and a working software product! Tanzu Labs often help with the development of Herald during 'beach time' between projects, and so have experience in using Herald. Tanzu Labs worked with Alberta Health to rearchitect the Alberta TraceTogether application on to Herald.
- [FaceDrive Health](https://health.facedrive.com/) - A contributor to Herald, this Canadian eHealth wearables and system manufacturer supplies wearables and a private company deployable Digital Contact Tracing solution. They can also supply wearables, beacons, and consultancy.
- [Proxximos](https://proxximos.com/) - A UK startup that specialises in private employee safety and health, including wearables for Digital Contact Tracing. They provide wearables and beacons. They can also provide consultancy on custom solutions.
- [Operation Outbreak](https://operationoutbreak.org/) - A STEM outreach educational platform from Massachusetts that uses Herald in a mobile app to pass on a 'virtual' virus between participants. This programme helps educate teenagers in public health and technology, aiming to get them interested in Science, Technology, Engineerming and Maths. This application was rearchitected for Herald in 2021 when their commercial Bluetooth API partner stopped supporting their proprietary Bluetooth API. They now use Herald opensource for free. They are also working with the Herald team to contribute a [Flutter User Interface API to Herald](https://github.com/theheraldproject/herald-for-flutter).

## Open source applications for Public Health

The Herald Project currently do not have a full end to end suite of applications for these use cases. We will develop the below applications soon:-

- A free mobile application on Apple and Google app stores to provision and manage individual Herald Venue beacons, or a MESH of beacons
- A free mobile application on Apple and Google app stores to track an individuals social mixing, record a Venue Beacon Diary, and [navigate inside hospitals](/uses/hospitals)
---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Herald Project Bluetooth Proximity Research
description: Herald Project Bluetooth Proximity Research Home
---

# Introduction

Herald provides reliable Bluetooth communication and range finding across a wide range of mobile devices, allowing
Contact Tracing and other applications to have regular and accurate information to make them highly effective.

|Wide Phone Support|Easy to integrate|Flexible|
|---|---|---|
|95% of phones worldwide can use the Herald Protocol|Provide a few callbacks to the Herald API and integrate Herald to your app in minutes using your own payload|Does not restrict how people can use it, for contact tracing or any reliable Bluetooth communication|

Herald provides a solution to the following epidemiological problems:-

![Herald epidemiological solutions](images/EstimationBenefits.png)

## Features

|<b>Detect nearby phones</b>|100% detection of phones in the foreground and background across iOS and Android devices. Herald supports 100% of the phones in the UK that support advertising, as well as the 35% of Android phones that cannot act as 'advertisers' and so remain unseen by advertising-only based protocols.|
|<b>Provide regular distance readings</b>|Herald performs distance estimations every few seconds, with higher frequency on modern phones. This allows for a more accurate data and risk picture over time. Maximum frequency can be configured to optimise battery use. At ~4s per reading battery use is 6-11% over 8 hours, depending on the age of the phone and its battery capacity.|
|<b>Interoperate internationally</b>|By providing a common packet header we allow for international interoperability amongst all contact tracing applications, whether designed for centralised or decentralised contact matching and risk scoring.|

## Latest blog posts

The last news can be found in our [blog](blog).

## Documentation

We have a large section on [Development & Integration](docs).

## Thanks

A lot of work has gone in to mobile app based contact tracing protocol 
research, design, testing and collaboration worldwide. We'd like to thank 
all of those in VMware Pivotal Labs and elsewhere worldwide that have 
assisted with various national and state governments to use mobile contact 
tracing to help save lives. ❤️

## License

All Herald works are Copyright 2020 VMware, Inc.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

The repositories for Herald (Herald, Android, iOS, Analysis Scripts, Calibration tool) are MIT licensed.

See LICENSE.txt and NOTICE.txt for details.

---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Bluetooth Proximity Research
description: Bluetooth Proximity Research Home
---

# Introduction

This site details the research done during Summer 2020 on mobile phone Bluetooth low-level performance and how this pertains to local communication between mobile devices.

The aim of this research is to provide a much more reliable way to perform local information interchange. This has many practical benefits, but the key one we're concerned with is the enable improved Bluetooth Proximity Detection to improved Contact Tracing applications worldwide in order to help stop the spread of COVID-19.

Our research has been split in several parts. We've also provided an independent introduction to mobile app based contact tracing and provide explanations of the key aspects of such a system. This aims to demystify the approach and increase public trust.

The remainder of this page breaks down the key areas of the documentation on this site.

## Blog

The last news can be found in our [blog](./blog).

## Introduction to mobile app based contact tracing

A [layman's introduction](./background) to contact tracing and mobile app proximity detection.

## Paper: Measuring the efficacy of mobile proximity protocols

A [scientific paper suggestion a fair and independent approach to measuring the efficacy of a mobile phone based contact tracing protocol](./paper), providing a direct calculation of efficacy that can be used with the Oxford Risk Model to calculate the extent to which such an approach can control the spread of a virtus in a given population.

## Comparing protocols: Formal testing results

A [list of results](./results) ourselves and others have calculated using our method for various existing proximity protocols used for contact tracing today.

## TBD NAME: Low-level bluetooth communication protocol

During our research on creating an efficacy measure we created a [Bluetooth based protocol](./protocol) implementation to test the ideas. We found a welath of issues in Bluetooth on mobile phones, identified the issues, and workaround them in order to provide a robust communication protocol. 

This low level protocol is independent of any information you pass over it. You could use it for contact tracing proximity detection, but also other uses within your own applications. This low-level protocol provides reliable communications between proximite mobile devices.

## TBD NAME: Privacy preserving payload that can counter hostile actors

We have created a design for a [payload](./payload) that not only preserves privacy, but also provides the right levels of epidemiological insight to prevent virus spread. We show this can be done in a way that ensures security of the contact tracing system, and can be trusted by people worldwide as they go about their lives. The protocol has international interoperability built in and prevents attack from hostile groups or state actors.

The protocol also supports other sample payloads. You can plug in your own for your specific application needs. See [Protocol Payloads](./protocol/payloads) for details.

## Bluetooth low level engineering issues

We found a lot of issues in mobile phone Bluetooth implementations and have worked around a great many of them. You can read about our [Bluetooth discoveries and workarounds here](./bluetooth).

## Thanks

A lot of work has gone in to mobile app based contact tracing protocol research, design, testing and collaboration worldwide. We'd like to thank all of those in VMware Pivotal Labs and elsewhere worldwide that have assisted with various national and state governments to use mobile contact tracing to help save lives. ❤️


---
layout: posts
title:  "Why Digital Contact Tracing is still needed"
date:   2021-06-25 09:00:00 +0100
categories: blog update jekyll
author_name: adamfowleruk
author_url: https://github.com/adamfowleruk
author_avatar: https://avatars2.githubusercontent.com/u/2700521?s=150&u=7998edeafa7e4a1bf65095b13c8a4fd49c240e84&v=4
---

We're now over a year since the first national lockdowns due to SARS-CoV-2 (COVID-19). In this post we look to the
continued need of Digital Contact Tracing (DCT), and where it is going next.

### The need for DCT

SARS-CoV-2 was unique due to having several features in a single disease:-

- High number of asymptomatic carriers (People spreading without showing signs of disease)
- Even once disease onset, symptoms were mild, but some succumbed to sudden deterioration
- Treatment of the seriously ill involved specialist equipemnt (ventilators) and took many days to recover before being released from hospital
- Long period of time until the onset of symptoms (mean of ~4.5 days, 95 percentile of 14 days)

These features in one disease meant that cases could slowly build over time and that manual contact tracers 
had their work cut out to trace a person's activities for up to two weeks rather than a couple of days.

It also meant that there was a very large burden placed on hospitals. Once hospital critical care beds
were filled it would mean an uptick in the number of deaths through the lack of specialist care.
Maximising those self isolating at home who had become ill became key to preventing onward spread.

This means that automated proximity measurement, exposure notification, and their linking to
manual contact tracing systems were needed in a way not required for previous pandemics.
This is where Digital Contact Tracing has emerged from.

A year later we're now seeing an onset of a Delta variant, and as I write this a new 
['Delta-Plus' (AY.1)](https://www.bbc.co.uk/news/world-asia-india-57564560) variant
is currently being tracked too - one which early indications show may be both more virulent and
resistent to vaccines.

Hopefully this early data will be proved wrong and vaccines will continue to provide a way out
of this pandemic, but we should take this opportunity to prepare and perfect our DCT approaches.

Whether in this or future pandemics, an approach will be needed to allow humanity to
respond to new emerging diseases or variants and respond quicker, minimising the impact on
both health services' ability to respond and the day to day freedoms of individuals.

### Putting Contact Tracing back into DCT

Due to the fast onset of SARS-CoV-2 many application teams sought a fully automated
DCT solution as manual teams were overwhelmed with the volume of cases.

Whilst useful in the peak of a pandemic, fully automated control without human 
intervention is not the default approach of contact tracing teams.

Contact Tracers use epidemiological knowledge and experience to tailor their 
recommendations to individuals. Each decision is based upon a conversation
and understanding of an individual case.

This has not been the case for automated Exposure Notification systems. Instead
they have operated, in most instances, as either a 'black box' - being allowed
to fully automate and recommend self isolation, or as a 'fill the gaps' solution
to an individual's contact sheet where they don't know the name of who they were near to,
say on public transport.

The Black Box Exposure Notification (EN) approach has been viewed with scepticism by
manual contact tracing teams as they do not have access to the basis of calculation
for individuals' decisions. It effectively operates as a system outside and separate
from the standard public health contact tracing approaches which have been honed 
over the past 100 years.

A Black Box approach also means no data is available to make decisions from until
a disease is understood, so that thresholds and policies can be set for self isolation
advice. This means at an emergence of a new pandemic these solutions will not
be enabled for full automation as they are not trusted, and they won't provide early
warning data to contact tracing teams and epidemiologists.

The 'fill the gaps' approach of Digital Contact Tracing has proved useful. In many
cases contacts that would have never been reached through manual contact tracing have
been reached through supplementing personal memories with automated contact matching.

The downside to this approach is that a person in advance has to share their identity
with the contact tracing system provider (typically a public health authority), and
so strong legal safeguards are often added to prevent abuse of this data by governments.

In addition, although rich exposure risk measurement data is now possible through
proximity detection mechanisms, such as Herald, this data has not been made available
to the manual contact tracing teams. They just see a list of extra contacts, and perhaps
an approximate number of 5-minute windows these individuals were near each other.

What is needed is a blending of these rough edges of privacy, data accuracy, and providing
public health experts with enhanced data. With this new diseases or strains can be 
identified early, enhanced data can be provided to public health teams, and trust
can be built into the analyses of DCT systems prior to enabling any automated
exposure advice.

This approach is Digitally Assisted Contact Tracing (DACT) and is something we're actively
seeking to develop in The Herald Project alongside our Project Advisory Group, which includes
PHAs and experts in DCT.

### How the Herald project will drive this effort

Over the course of the summer of 2021 The Herald Project will look to create
a white-labled end-to-end DCT/DACT and Exposure Notification system built on top
of the Herald Proximity library.

Those countries and states without such systems will be able to quickly take our
opensource software and use it, to the benefit of their communities. This helps
protect countries who cannot afford a hundred person application development team.

Those with existing systems will be able to take individual modules and plug them
into their existing DCT system - no matter which Bluetooth protocol they are
using between devices. Interoperability will be key.

We will also publish the reference implementation of the 
[Herald Interoperability Protocol]({{"/specs/payload-interop" | relative_url }})
allowing countries running the same or separate systems to exchange exposure information
in a privacy preserving way once borders re-open.

Our first step in this journey will be to complete the first version of our wearable
opensource hardware to provide reliable DCT and, optionally, biomonitoring to individuals
who cannot afford the latest thousand dollar smartphones that are currently required to
run many DCT applications today.

After this we'll create an whitelabled DCT mobile app and a DCT healthcare
backend, allowing PHAs to rapidly publish their own applications and systems based on
this well developed and tested foundation.

### How you can help

We're community driven, 
so please come and [join the Herald community]({{"/community" | relative_url }}) and help us with deciding the
future of the Herald project!
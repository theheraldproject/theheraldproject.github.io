---
layout: posts
title:  "Bluetooth RSSI proximity self calibration enhancements"
date:   2022-03-11 07:00:00 +0100
categories: blog update jekyll
author_name: adamfowleruk
author_url: https://github.com/adamfowleruk
author_avatar: https://avatars2.githubusercontent.com/u/2700521?s=150&u=7998edeafa7e4a1bf65095b13c8a4fd49c240e84&v=4
---

# Herald announces working approach to Bluetooth phone RSSI proximity self calibration

Today The Herald Project, part of LFPH, announces a promising new approach to
measuring phone-to-phone proximity accurately using Bluetooth Low Energy on
consumer mobile phones.

A problem since the start of the SARS-CoV-2 pandemic has been accurately measuring
proximity using mobile phones. The most prevalent sensor available is your phones Bluetooth
chip and antenna. 

The Herald Project has researched the variability of RSSI signal strength data and published
millions of rows of datasets down to 1cm resolution between different phones to research the
possibilities here.

We have recently completed this research by creating a mechanism allowing for phones to self-calibrate
- removing the variation from how different people store and use their phones by calibrating based
on their behaviour - their close contacts with other people and how humans socialise with each other.

When you talk to your close friends you adopt a particular distance. For acquaintences slightly further, and so on.
Using this information for a subset of your most prolonged 'contact events' (where your phone detects others)
has enabled us to devise a way for your phone to calibrate to a standard scale, allowing proximity to be calibrated
to your behaviour over time.

This works by correcting for Transmit Power from remote phones and filtering real interactions over a period of time (2.5 days should be sufficient) and then
spotting peaks in corrected data to infer 'close/personal space' and 'near/acquiantance space' using Proxemics, and calibrating
a proximity scale to those human behavioural zones.

Once the data is calibrated, epidemiologists can create risk algorithms that use this standard
proximity scale.

Using two different phones on test subjects that live together and have very different jobs and working hours showed that 
calibrating on all other Bluetooth contact events, then applying each phones own calibration results to their mutual contact events, 
gave very similar risk scores - to within 7.5% of each other. (More accurate than your step counters when you go walking with a friend!)

We have also provided a file - heraldrisk.R - under an Apache 2.0 license allowing others to use our Herald demo app
or these functions independently to calibrate their own devices.

More can be read about this on our GitHub site: https://github.com/theheraldproject/herald-analysis#phone-self-calibration-scripts

The Herald Project will now work to incorporate this into our next release of the Herald Risk and Exposure API,
allowing Public Health Authorities to build this new accurate risk model with 7 second resolution into their
COVID-19 Digital Contact Tracing apps, and to provide for the first time a 'Social Mixing Score' feedback to 
a user.

Providing social mixing scores on apps will allow people to measure and track their own social mixing
in much the same way as people monitor their daily step counts. They'll be able to have a consistent
points score per day for mixing with other people, and choose how to modify their own behaviour
to minimise their risk of contracting disease.

This crucially allows them to prevent themselves being notified of exposure rather than wait 
to be told they may have received too much exposure over the past few days. This should
reduce the spread of disease and do so by reducing the number of people we ask to self
isolate via COVID-19 apps.


## Get Involved

The Herald Project thrives on community participation. If you want to get involved, or have a great idea for improving healthcare, please get in touch. Visit the [Herald Community Page]({{"/community" | relative_url }}).

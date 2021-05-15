---
layout: posts
title:  "Why measuring Distance and Risk accurately is important"
date:   2021-05-17 10:00:00 +0100
categories: blog update jekyll
author_name: adamfowleruk
author_url: https://github.com/adamfowleruk
author_avatar: https://avatars2.githubusercontent.com/u/2700521?s=150&u=7998edeafa7e4a1bf65095b13c8a4fd49c240e84&v=4
---

Herald allows for very regular Bluetooth RSSI readings. These can be used to estimate distance. In this blog we discuss
why this is important, and what this information can be used for, and how we provide an accurate way of doing this in Herald.

### Uses of distance estimation

The Herald Proximity API is designed to do two things very well:-

- Provide regular information on the presence of other local devices (E.g. those running Herald services, or a DCT app)
- Reliably exchange payload (application data) between these devices

Up until version v1.3 Herald has only provided low-level information on nearby devices. For Bluetooth Low Energy this
is the regular, raw RSSI (Signal Strength) information calculated by your device about other nearby devices. 
From v1.3, the Analysis API allows you to choose from a variety of algorithms to convert this data to a distance estimate.

Beyond simple 'nearby' proximity information, 'Distance' estimation is useful in a variety of applications:-

- Triangulating a device's position within a fixed-beacon or Bluetooth MESH environment to facilitate accurate in-building navigation
- Attaching Bluetooth beacons to high-value devices, such as in a hospital setting, to locate valuable and urgently needed equipment
- Measuring an employee's exposure in terms of distance and duration to a risky work environment, such as exposure to chlorine or excessively noise in an industrial setting
- Digital Contact Tracing - to more accurately calculate 'exposure risk' between people in complex environments as they move around

### Important aspects for distance estimation

Much research has been done on using occassional RSSI samples to perform distance estimation. 
Many of these studies have - incorrectly - concluded that Bluetooth RSSI cannot be used for accurate distance estimation, as we'll show below.

There are a variety of reasons for these conclusions:-

- Many studies used occassional RSSI samples rather than every advertisement's RSSI value across every advertising channel (this makes the RSSI value appear to 'jump around')
- Many existing distance conversion algorithms use 'device calibrated at a single fixed distance' approaches
- No current mechanism takes advantage of the fact BLE advertisements are required to be done on all three channels, which have different frequency separations

The Herald Project team have discovered that a reliable RSSI value can be achieved if you take every RSSI from every advertisement of a nearby device. There is a clear modal value for a device at a fixed distance once a few samples are taken.

What about moving devices, and devices with very different transmit powers? Interestingly, we have discovered that at ranges between 25cm and 800cm - the ideal range
for Digital Contact Tracing - the shape of the RSSI chart is consistent between devices, even if the RSSI value ranges are different. 
The interference pattern across the three advertising channel frequencies has a dominant effect on RSSI calculations at ranges below 8 metres.

This chart does fluctuate rather than follow an exact linear or log scale, but the pattern of fluctuations is always consistent at these ranges. We've measured a set
of devices at 1cm intervals to determine this.

We've also determined that the fact that the three advertising channels have different frequency separation - and the fact we know that each advertisement is always
given on all three channels - allows you to construct a multi-variate algorithm to determine where you are on that curve. So long as the two devices are not introduced at a static distance and never move, and they move by at least 7cm, you can determine at what distance the phones are at. You don't even need to perform this separation though - the fact it happens predictably can be used with a simple linear model at short distances to give a good distance estimate.

We'll provide more information on these aspects in a full section of the website, but for now you just need to know this: In order to provide accurate distance estimation in Bluetooth Low Energy you need to:

- Take every BLE Advertisement packet for each nearby device and their RSSI values - don't use an occasional single sample (most devices advertise between 1 and 5 times a second on all three channels)
- Use a set of recent RSSI values to calculate a distance estimation
- For risk measurements derived from this, make the time slices for risk 'slice' calculation as small as possible

### The Herald Analysis API

In v1.3 we are providing a fixed-memory use streaming Analysis API. This maintains a fixed size sample list for each measurement for each nearby device. The output from one analysis algorithm can become the input of another. (Note: We use the term 'sample list', but remember - we provide ALL raw RSSI values, we don't sample them.)

So for Digital Contact Tracing (DCT), for example, we expose the raw RSSI data to a distance conversion algorithm. 
New raw samples are taken as they arrive from Herald's Bluetooth layer and added to this fixed size 'sample list' which maintains the last X most recent samples. 
This size is configurable by app developers. This produces a Distance value added to a distance sample list which is in turn exposed to a risk calculation algorithm. 


(Remember too, that Herald is transport independent. Whilst our reference implementation today is Bluetooth Low Energy, in future we'll support other transports, including Ultra Wide Band (UWB) radio).

Every so often the Analysis API's algorithms will be invoked over the current sample lists. This will produce a new distance value from the several recent RSSI values which is added to the distance list. The recent distance values then are used to calculate risk exposure 'slices'.

In a default implementation we'll carry out new analysis runs every 5 seconds - giving a very small 'slice' to be added to a Risk exposure value. Application developers can customise this time interval to produce even thinner risk slices if they wish.

### What this means for accuracy

Herald now provides in v1.3 the most accurate and efficient way of calculating both distance and risk through its API of any Digital Contact Tracing or Proximity system for Bluetooth Low Energy. Epidemiologists can provide their own risk algorithms or simply change the configuration parameters of the sample ones we provide. The Logarithmic risk algorithm provided, for example, would allow you to use the UK's Oxford Risk Model for COVID-19 with the Herald API given the right parameters.

In our testing we have found that using a self-calibrated distance mechanism provides accuracy of around 10cm at ranges between 25cm and 800cm. When you consider that dedicated technologies like Ultra Wide Band (UWB) radio has an accuracy of 5-7cm, and most existing BLE distance approaches are accurate to between 50-200cm, Herald provides quite an upgrade to distance estimation!

### Where to go from here

You can take one of the existing distance and risk conversion algorithms and trial it yourself. Our demonstration app for iOS and Android now shows both raw RSSI and distance estimate values for the basic smoothed linear model. This is a self-calibrating model that becomes more accurate over time when used in a real environment with other phones. The model assumes a phone is carried by the user, therefore it should experience the full range of distances over time for calibration. 

The default parameters assumes a typical phone will spends 8 hours/day within 3.7m of other phones (i.e. at work), and up to 5 minutes/day within 0.5m of other phones (i.e. passing in the street). As such, a quick test with two static phones on a desk will show wildly incorrect absolute distance estimates (e.g. 0.2m showing as 3m), however the relative estimates shall remain valid for detecting whether the phones have moved closer or further apart. 

This model gets more accurate the more the phone is used with more other phones - so use it with your friends, family, and colleagues for a day or so in real world conditions before exclaiming about it's accuracy!

The Analysis API is fully pluggable though, so feel free to come up with your own distance and risk algorithms and try them out. The API is very simple, and we'll provide an Analysis API developers guide in v2.1.

v2.1 will also include a new Exposure API. This will be built on top of the Analysis API as a consumer of its 'Risk' samples, but also provide a pluggable mechanism to talk to Public Health Authority backends in order to exchange decentralised or centralised exposure token information. Importantly, this will include support not only for Herald tokens, but also BlueTrace (OpenTrace) and GAEN (Google-Apple EN API). This means that even if your national app uses another protocol like GAEN, plugging in the Herald API will allow you to use your own more detailed algorithms for your epidemiological needs.

v2.1 will also support multiple diseases and strains and multiple risk models for Exposure alerts for Infection Prevention and Control (IPC), with configurable 'risk thresholds' and advice triggering rules for each. We want to ensure that humanity is prepared for a future pandemic, and want to also provide something useful to stop other outbreaks, such as Ebola.

### Get involved

We're community driven, so please come and [join the Herald community]({{"/community" | relative_url }}) 
and help us build the future of the Herald project!

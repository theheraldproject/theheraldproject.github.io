---
layout: docs
title: Wearables
description: Herald's use for healthcare wearables
menubar: about_menu
---

# Herald and Wearables

Herald has a [C++ API and demo applications](https://github.com/vmware/herald-for-cpp) that showcase
how a low-cost but high performance eHealth wearable can be developed.

Our first wearable sample application is for 
[Digital Contact Tracing (DCT)]({{"/applications/ct" | relative_url }})
and provides the same DCT functionality as our iOS and Android Herald API and sample applications.

This also works well with our Venue Beacon, logging both nearby contacts and when you visited
restaurants and bars - both check-in and check-out time - and storing them on your wearable.
You can then choose to later share these with contact tracers.

## Opensource hardware - COMING SOON!

We are also working on an opensource hardware design that will be published shortly in Herald v1.5.

The focus of this design is to minimise the cost of producing DCT solutions for use by those
who cannot afford $1000 smartphones, for young and old people without this technology, and
for parts of the world without the infrastructure to support a traditional DCT system.

You can see an early mock up of our hardware design below:-


<div class="row">
<div class="md-4">
<img src="../images/dct-wearable-prototype.png" alt="Herald early wearable design mock up" style="width: 60%;"/>
<br/>
<b>Note:</b> An early mock-up of the main module of our DCT wearable.
<br/>
<br/>
</div>
</div>
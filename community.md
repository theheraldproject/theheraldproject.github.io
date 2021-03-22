---
layout: docs
title: Community
description: Herald Community
id: community
---
Do you want to help build Herald?

If you’re a newcomer, check out the “[Good first issue](https://github.com/vmware/herald/issues?q=is%3Aopen+is%3Aissue+label%3A%22Good+first+issue%22)” and “[Help wanted](https://github.com/vmware/herald/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3A%22Help+wanted%22+)” labels in the Herald documentation repository.

* Join the Linux Foundation Public Health's Slack and our #herald-general channel and talk to our community members: <a href="https://slack.lfph.io" target="_new">https://slack.lfph.io/</a>

* Follow us on Twitter at [@HeraldBluetooth](https://twitter.com/HeraldBluetooth)

## Helps

Below are some items the Herald project team are looking for specific assistance with. If you feel you can help then please visit the links below:-

|Help Summary|Ideal timeline|Links|
|---|---|---|
|**DCT Distance conversion and Risk Exposure algorithm design, implementation, and evaluation**<br>The Herald project have published [2.3 million data points]({{"/blog/rssi-data" | relative_url }}) from phones at 1cm resolution and are working on distance conversion routines to make them more accurate. We've also provided a general analysis streaming API to enable this and various disease exposure estimation routines to be created and implementation on mobile devices. If you would like to help us implement and evaluate these methods please come talk to us on Slack|Apr-Sep 2021|[API Sample tests](https://github.com/vmware/herald-for-cpp/blob/b8bf70a208751a9727409f000ded5c27a78f55e3/herald-tests/ranges-tests.cpp#L321)|
|**Website information architecture**<br>We need someone who is experienced at helping a very broad community with different interests to find information on our website. We're improved it recently, but more work still is needed. Please visit our #herald-general slack channel on the LFPH slack instance detailed above|Any time|This site!|
|**Pseudo Device Address randomness methods**<br>Some phones incorrectly rotate their BLe MAC address every few seconds. This causes device discovery routines to reconsider a device even though it may have only just been identified recently. The Pseudo Device Address is a method around this. The mechanism used to generate this address needs to be reliable on a wide range of phones - even when idle for many hours - and prevent a passing attacker from predicting future addresses based on just the current observable address. There are many methods - we're looking for suggestions that work across Android devices going back, ideally, to Android 5.0.|Early-Mid Jan 2021|[Android GitHub Issue #118](https://github.com/vmware/herald-for-android/issues/118)|

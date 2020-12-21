---
layout: page
title: Community
description: Herald Community
id: community
---
Do you want to help build Herald?

If you’re a newcomer, check out the “[Good first issue](https://github.com/vmware/herald/issues?q=is%3Aopen+is%3Aissue+label%3A%22Good+first+issue%22)” and “[Help wanted](https://github.com/vmware/herald/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3A%22Help+wanted%22+)” labels in the Herald documentation repository.

* Join our Herald Bluetooth Slack and talk to our mobile Bluetooth and Contact Tracing community members: <a href="https://herald-bluetooth.slack.com/" target="_new">https://herald-bluetooth.slack.com/</a>

* Follow us on Twitter at [@HeraldBluetooth](https://twitter.com/HeraldBluetooth)

## Helps

Below are some items the Herald project team are looking for specific feedback on. If you feel you can help then please visit the links below:-

|Help Summary|Ideal timeline|Links|
|---|---|---|
|**Pseudo Device Address randomness methods**<br>Some phones incorrectly rotate their BLe MAC address every few seconds. This causes device discovery routines to reconsider a device even though it may have only just been identified recently. The Pseudo Device Address is a method around this. The mechanism used to generate this address needs to be reliable on a wide range of phones - even when idle for many hours - and prevent a passing attacker from predicting future addresses based on just the current observable address. There are many methods - we're looking for suggestions that work across Android devices going back, ideally, to Android 5.0.|Early-Mid Jan 2021|[Android GitHub Issue #118](https://github.com/vmware/herald-for-android/issues/118)|

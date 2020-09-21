---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Outer payloads
description: Custom outer payload design
menubar: docs_menu
---

# Custom outer payloads

If you have implemented a [Commercial application](/protocol/commercial) using the Squire protocol
then you may wish to completely change what data is passed between devices.

The [Squire Envelope Payload](/payload/envelope) is an example of such a payload.

Your outer payload may be an envelope payload with multiple wrapped payloads, or a full payload
custom to your application.

## How to implement an Outer payload

This is simply a case of plugging in your own Payload provide directly to the Squire Protocol.
You can still use the Squire Protocol even for a completely new payload. Callbacks are provided
to customise its behaviour. See [the integration guide](/guide) for details.

## Wrapping an inner protocol

You can either provide just an outer payload or perform a similar outer header approach that
we adopt. See the [Squire Envelope Header page](/payload/envelope) for more details on that
approach.

## Interoperability

Because your payload is completely custom, do not forget to use your own Service and Characteristic UUIDs
for the Squire Protocol. This is described on the [Commercial Protocol](/protocol/commercial) page.

Doing this will stop your app interfering with others' apps, and prevent those apps from interfering with
yours.

## Share your work!

We will happily list and mention your payload, whether commercial or open source, on this site. Please
provide high level details of your payload and a website link for further details via a 
[GitHub Issues](https://github.com/vmware/squire/issues) request.
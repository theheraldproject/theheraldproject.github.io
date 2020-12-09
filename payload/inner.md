---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Inner payloads
description: Custom inner payload design
menubar: docs_menu
---

# Custom inner payloads

A custom inner payload is designed to work exclusively with the
[Herald Envelope payload]({{"/payload/envelope" | relative_url }}). If you want a completely
custom payload, see the [custom outer payload page]({{"/payload/outer" | relative_url }}).

The inner payload is constructed by a national/state authority and MAY only be
verifiable or readable by that health authority.

There are many reasons individual countries' inner packets may vary:-

- A different appetite for privacy vs national security, and thus more data needing to be shared
   - E.g. one country may be decentralised, another centralised, but using the [Herald Envelope payload]({{"/payload/envelope" | relative_url }}) would allow for international interoperability still
- Requiring the sharing of more epidemiologically useful information (E.g. phones were outside, phone orientation, GPS location)

Note: We recommend the [Herald Secured inner payload]({{"/payload/secured" | relative_url }}) as this provides full epidemiological information
as well as ensuring national security and individual privacy.

## How to implement a custom inner payload

There are only two steps:-

- Implement a Payload provider callback to construct your inner payload on the fly
- Provide size information on the inner payload by either:-
  - If using a fixed payload size, tell the Herald protocol what your inner payload size is
  - If using a variable payload size, implement a Payload Parser callback

Performing these steps allows your national contact tracing app to continue as-is, but supporting international interoperability by using
the [Herald Envelope payload]({{"/payload/envelope" | relative_url }}).

## Alternatives

Alternatively, implement a [custom outer and inner payload]({{"/payload/outer" | relative_url }}).

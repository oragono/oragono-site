---
title: oragono.io/nope Capability
layout: spec
meta-description: A capability that cannot be requested
copyrights:
  -
    name: "Daniel Oaks"
    period: "2019"
    email: "daniel@danieloaks.net"
  -
    name: "Shivaram Lingamneni"
    period: "2019"
    email: "slingamn@cs.stanford.edu"
---


## Introduction

New IRCv3 features are typically designed for backwards compatibility with legacy clients. One of the principal mechanisms for supporting this is [capability negotiation](https://ircv3.net/specs/core/capability-negotiation.html). Servers advertise a list of optional features, and clients can request subsets of them to be enabled on their connections.

Clients should only request capabilities which they have explicit provisions for supporting; they should not blindly request capabilities that the server advertises, since one should not assume that capabilities will change the server behavior only in benign, backwards-compatible ways (e.g., by adding another tag). `oragono.io/nope` is a capability, inspired by the [GREASE](https://tools.ietf.org/html/draft-ietf-tls-grease-01) mechanism for TLS, that attempts to discourage clients from doing so.

## The Capability

The `oragono.io/nope` capability has no value. If a client requests it during registration, the server MUST send an `ERROR` message with a suitable description (e.g., "Requesting the oragono.io/nope capability is forbidden") and close the connection. If a client requests it outside of registration, the server MUST NOT send an `ACK` for it, but SHOULD NOT disconnect the client, since this might be disruptive to end users.

## Future work

In the future we may consider randomly generating the capability name (by analogy with the GREASE specification).

---
title: oragono.io/maxline-2 Capability
layout: spec
meta-description: A method for negotiating longer IRC protocol lines.
copyrights:
  -
    name: "Daniel Oaks"
    period: "2016-2019"
    email: "daniel@danieloaks.net"
---
## Introduction
There has been a desire to allow longer lines while sending or receiving IRC traffic, allowing longer messages with `PRIVMSG`/`NOTICE` and doing away with (or at least working around) the original limit of 512 bytes. However, doing so in a backwards-compatible way requires opt-in from clients and explicit action from servers.

The `oragono.io/maxline-2` capability specifies the maximum length of the 'rest' of the message (everything that isn't the tags section), that the server allows. Lines are truncated or split to account for clients which have not negotiated this extended length.


## Architecture

### The Capability
The `oragono.io/maxline-2` capability, when advertised, MUST have a value which is a positive integer.

For example:

    C: CAP LS 302
    S: CAP * LS :oragono.io/maxline-2=2048

To match standard message handling, tags and the rest of the message have separate length values. The value of this capability represents the maximum number of bytes that the 'rest' section of the message can take up.

As an example, if `maxline` is `1024` then the maximum size of a full IRC message would be `<taglength>+1024` bytes (the standard [length of the tags section](https://ircv3.net/specs/extensions/message-tags.html#size-limit) plus 1024 for the rest of the message).

Servers MUST truncate incoming messages to the `maxline` value before processing them.

The `maxline` value MUST be at least `512` and SHOULD default to at least `2048`.

### Interaction with PRIVMSG and NOTICE
If a client has negotiated the `oragono.io/maxline-2` capability and sends a `PRIVMSG` or a `NOTICE` message that is longer than 512 bytes, the receiving server MUST split this into multiple regular length messages (maximum 512 bytes) when sending it to clients that have not negotiated this capability.

Servers SHOULD split on whitespace or other word boundaries, but may use whatever method is easiest for them to implement. Lines SHOULD NOT be split in the middle of a UTF-8 character.

Servers MAY split other commands/numerics into multiple lines in whatever way ensures the best compatibility and functionality.

### The `oragono.io/truncated` Tag
The `oragono.io/truncated` tag, when present, indicates that a message has been truncated due to the client's line length. It may be sent to any client which has negotiated the [`message-tags` capability](https://ircv3.net/specs/extensions/message-tags.html#message-tags-capability).

As an example, this tag may be used when a long channel `TOPIC` is set, but cannot be relayed entirely to a user due to its length.

### Examples
In the following examples, the default message length is presumed to be `80` (rather than `512`) and the extended length is presumed to be `400`. This is done purely to simplify presentation and convey the concept more easily.

* ` ->` conveys lines being sent from this client to the IRC server.
* `<- ` conveys lines being sent from the server to this client.

#### Sending a long privmsg to someone else
In this example, C1 and C2 have negotiated `oragono.io/maxline-2`.

    C1  -> PRIVMSG coolfriend :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.
    C2 <-  :c1!test@localhost PRIVMSG coolfriend :Lorem ipsum dolor sit amet, consectetur
           adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

In this example, C1 has negotiated `oragono.io/maxline-2` but C2 has not.

    C1  -> PRIVMSG coolfriend :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.
    C2 <-  :c1!test@localhost PRIVMSG coolfriend :Lorem ipsum dolor sit amet, consectetur
    C2 <-  :c1!test@localhost PRIVMSG coolfriend :adipiscing elit, sed do eiusmod tempor
    C2 <-  :c1!test@localhost PRIVMSG coolfriend :incididunt ut labore et dolore magna aliqua.

#### Setting a long topic and having another user run into it
In this example, C1 and C2 have negotiated `oragono.io/maxline-2`.

    C1  -> TOPIC #coolchan :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.
    C1 <-  332 c1 #coolchan :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.

    C2  -> TOPIC #coolchan
    C2 <-  332 c1 #coolchan :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.

In this example, C1 has negotiated `oragono.io/maxline-2` but C2 has not.

    C1  -> TOPIC #coolchan :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.
    C1 <-  332 c1 #coolchan :Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
           eiusmod tempor incididunt ut labore et dolore magna aliqua.

    C2  -> TOPIC #coolchan
    C2 <-  @oragono.io/truncated 332 c1 #coolchan :Lorem ipsum dolor sit amet,


## Implementation Considerations
This capability changes a pretty fundamental property of IRC messages (the length of them). As such, there are considerations which implementers should keep in mind. This section lists those non-normative considerations and suggestions.

### Rate Limiting
Servers should carefully consider their rate-limiting policies when implementing this extension. For example, with a rate-limiter based on penalties applied to each client, where the server currently applies one rate-limiting penalty for messages 512-bytes and under, they may apply multiple rate-limiting penalties for larger messages to compensate.

This is particularly useful on the `PRIVMSG` and `NOTICE` commands, where the required message splitting may increase the workload on servers by a significant amount for larger messages.

### Denial-of-Service
Most current algorithms and parsers are tuned and intended specifically to be used with 512-length messages. This extension allows servers to vastly increase these limits. As such, server, client, and services authors should all take a very close look at the algorithms and message-parsing code while investigating this extension. In particular, certain algorithms may overflow or or present degraded performance when dealing with these longer lines.

Clients may find it useful to have a maximum limit on the message size they will negotiate.

### Registration and Authentication
For server and services authors, one area to consider is the length of credentials accepted during registration. For example, if a client registers an account on a client that has negotiated `oragono.io/maxline-2`, they may have trouble authenticating later due to the length of their credentials. Servers and/or services should work to prevent situations like this (for example, by using SASL with a large number of allowed continuation messages).


## History

### Changes from oragono.io/maxline
The new message-tags spec boosts the tag space to 8191 bytes, which is much larger than the previously-allowed 512 bytes. Because it's no longer necessary, and because having our own tag-extension method on top of the new limit makes things very difficult for software, `oragono.io/maxline-2` no longer extends the tag limit and just focuses on the 'rest' portion.

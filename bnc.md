---
title: oragono.io/bnc Capability
layout: spec
meta-description: A method for attaching multiple clients to the same nickname
copyrights:
  -
    name: "Shivaram Lingamneni"
    period: "2019"
    email: "slingamn@cs.stanford.edu"
---
## Notes for implementing work-in-progress version
This feature is experimental. The capability name is also experimental and may change in future.

## Introduction
The fundamental identifier in IRC is the nickname. Traditionally, nicknames are ephemeral and are claimed upon connecting to the server: one can assume any unclaimed nickname, and closing or losing one's connection surrenders one's claim over the nickname. At most one client can be connected under a given nickname at a time.

This situation has fundamental deficiencies that have led to a patchwork of workarounds. For example, nicknames cannot be trusted to identify users: the conventional workaround for this is a service framework (i.e., a `NickServ` implementation) that can accept authentication requests and prevent unauthenticated connections for claiming reserved nicknames. Another issue is that connecting to an IRC network from a new location requires a new nickname, whose channel memberships and direct messages are not linked to any other nicknames. To remedy this, people use [bouncers](https://en.wikipedia.org/wiki/BNC_%28software%29), that is, proxy servers which initiate connections to an underlying IRC server, then accept multiple client connections and multiplex the underlying server connection over them.

With Oragono, we are planning a ground-up remedy for these problems, consisting of several independent features that can be deployed separately or together. Some are still planned, but some are already implemented. In particular, Oragono natively supports attaching multiple clients to the same nickname. This page describes that functionality and the associated IRCv3 capability, `oragono.io/bnc`.


## Behavior
When advertised, the `oragono.io/bnc` capability indicates that the server is capable of attaching multiple clients to the same nickname. Requesting the capability indicates that the client is willing to reattach to an existing nickname. That is to say, if a client authenticates with SASL and negotiates this capability during pre-registration, then requests a nickname which is already held by a different client authenticated to the same account, the server SHOULD grant the same nickname to the new client, instead of responding with `433 ERR_NICKNAMEINUSE`. It will then play the usual burst of registration messages corresponding to that nickname, then accept input from the client as usual.

If this happens, the following state MUST be shared between the two clients:

1. Channel memberships (at the conclusion of the registration burst, `JOIN` lines should be sent for any existing channels)
1. User and channel modes
1. Direct messages

However, the second client MUST NOT share its capabilities with the first: each client negotiates capabilities independently.

The server MAY provide this functionality even when the capability is not requested (in Oragono, this is configurable by the server administrator). If so, the server SHOULD provide a way for clients to opt out of it (in Oragono, `/msg NickServ set bouncer off`). Conversely, if the functionality is not provided by default, the server should provide a way for clients to opt into it without having to request the cap (in Oragono, `/msg NickServ set bouncer on`).

The server MUST verify that the two clients belong to the same user. Typically, this involves both clients authenticating with SASL during registration. Other mechanisms may be possible.

The server SHOULD NOT allow clients to mix TLS and non-TLS connections on the same nickname.

## Client support
Most clients support this behavior reasonably well, because it matches the longstanding behavior of bouncers. Some clients may have trouble interpreting certain messages from other sessions, in particular ["self messages"](https://defs.ircdocs.horse/info/selfmessages.html) (direct messages sent from a different client attached to the same nickname, to a different user).

Some clients support [znc.in/playback](https://wiki.znc.in/Playback), which will allow them to receive selective history playback upon reattaching.

Clients may wish to enable strict nickname reservation in tandem with this functionality, i.e., prevent anyone not authenticated to their account from using their nickname at any time. Depending on server configuration, this may either be the default, or available via a `NickServ` preference: `/msg NickServ set enforce strict` should be informative.

## Future work
The IRCv3 community is at work on additional specifications that will improve this functionality. In particular:

1. The [CHATHISTORY specification](https://github.com/ircv3/ircv3-specifications/pull/349) will make it easier for clients to receive selective history playback when reattaching.
1. Work is also in progress on a `BOUNCER` specification, which will enable clients to communicate to each other that messages have been read.

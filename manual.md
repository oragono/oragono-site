---
title: Manual
layout: default
meta-description: Manual for the Oragono IRC server.
---
{% include header.html %}
<div style="margin-top: -1em;">&nbsp;</div>

                                    Oragono IRCd Manual 2019-02-23
                                         https://oragono.io/

<em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Copyright © 2018 Daniel Oaks &lt;daniel@danieloaks.net&gt;</em>


{% include hrsec.html %}


# Table of Contents

- Introduction
    - Project Basics
- Installing
    - Windows
    - macOS / Linux / Raspberry Pi
- Features
    - User Accounts
        - Nickname reservation
    - Channel Registration
    - Language
- Frequently Asked Questions
- Modes
    - User Modes
    - Channel Modes
    - Channel Prefixes
- Commands
- Integrating with other software
    - HOPM
- Acknowledgements


{% include hrsec.html %}


# Introduction

This document goes over the Oragono IRC server, how to get it running and how to use it once it is up and running!

If you have any suggestions, issues or questions, feel free to submit an issue on our [GitHub repo](https://github.com/oragono/oragono/) or ask in our channel [`#oragono` on Freenode](ircs://irc.freenode.net:6697/#oragono).


## Project Basics

Let's go over some basics, for those new to Oragono. My name's Daniel, and I started the project (it was forked off a server called [Ergonomadic](https://github.com/edmund-huber/ergonomadic) that'd been around for a number of years). In addition to Oragono, I also do a lot of IRC specification work with the [various](https://modern.ircdocs.horse) [ircdocs](https://defs.ircdocs.horse) [projects](https://ircdocs.horse/specs/) and with the [IRCv3 Working Group](https://ircv3.net/).

Oragono's a new IRC server, written from scratch. My main goals when starting the project was to write a server that:

- Is fully-functional.
- I can use to very easily prototype new [IRCv3](https://ircv3.net/) proposals and features.
- I can consider a reference implementation for the [Modern spec](https://modern.ircdocs.horse).

All in all, these have gone pretty well. The server has relatively extensive command coverage, it prototypes a whole lot of the IRCv3 proposals and accepted/draft specs, and we pretty regularly update it to match new behaviour written into the Modern spec.

Some of the features that sets Oragono apart from other servers are:

- Extensive IRCv3 support.
- Extensive logging and oper privilege levels configuration.
- Integrated user account and channel registration system (no services required!).
- Native Unicode support (including appropriate casemapping).
- Support for [multiple languages](https://crowdin.com/project/oragono).


{% include hrsec.html %}


# Installing

In this section, we'll explain how to install and use the Oragono IRC server.


## Windows

To get started with Oragono on Windows:

1. Make sure you have the [latest release](https://github.com/oragono/oragono/releases/latest) downloaded.
1. Extract the zip file to a folder.
1. Copy and rename `oragono.yaml` to `ircd.yaml`.
1. Open up `ircd.yaml` using any text editor, and then save it once you're happy.
1. Open up a `cmd.exe` window, then `cd` to where you have Oragono extracted.
1. Run `oragono.exe mkcerts` if you want to generate new self-signed SSL/TLS certificates (note that you can't enable STS if you use self-signed certs).

To start the server, type `oragono.exe run` and hit enter, and the server should start!


## macOS / Linux / Raspberry Pi

To get started with Oragono on macOS, Linux, or on a Raspberry Pi:

1. Make sure you have the [latest release](https://github.com/oragono/oragono/releases/latest) for your OS/distro downloaded.
1. Extract the tar.gz file to a folder.
1. Copy and rename `oragono.yaml` to `ircd.yaml`.
1. Open up `ircd.yaml` using any text editor, and then save it once you're happy.
1. Open up a Terminal window, then `cd` to where you have Oragono extracted.
1. Run `./oragono mkcerts` if you want to generate new self-signed SSL/TLS certificates (note that you can't enable STS if you use self-signed certs).

To start the server, type `./oragono run` and hit enter, and the server should be ready to use!

If you're using Arch Linux, you can also install the [`oragono` package](https://aur.archlinux.org/packages/oragono/) from the AUR.


## Running oragono as a service on Linux

The recommended way to operate oragono as a service on Linux is via systemd. This provides a standard interface for starting, stopping, and rehashing (via `systemctl reload`) the service. It also captures oragono's loglines (sent to stderr in the default configuration) and writes them to the system journal.

If you're using Arch, the abovementioned AUR package bundles a systemd file for starting and stopping the server. If you're rolling your own deployment, here's an [example](https://github.com/darwin-network/slash/blob/master/etc/systemd/system/ircd.service) of a systemd unit file that can be used to run Oragono as an unprivileged role user.

On a non-systemd system, oragono can be configured to log to a file and used [logrotate(8)](https://linux.die.net/man/8/logrotate), since it will reopen its log files (as well as rehashing the config file) upon receiving a SIGHUP.


{% include hrsec.html %}


# Features

In this section, we'll explain and go through using various features of the Oragono IRC server.


## User Accounts

In most IRC servers you can use `NickServ` to register an account. You can do the same thing with Oragono, by default, with no other software needed!

These are the two ways you can register an account:

        /QUOTE ACC REGISTER <username> * passphrase :<password>
        /NS REGISTER <username> * <password>

This is the way to go if you want to use a regular password. `<username>` and `<password>` are your username and password, respectively (make sure the leave that one `:` before your actual password!).

        /QUOTE ACC REGISTER <username> * certfp *
        /NS REGISTER <username>

If you want to use a TLS client certificate to authenticate (`SASL CERTFP`), then you can use the above method to do so. If you're not sure what this is, don't worry – just use the above password method to register an account.

Once you've registered, you'll need to setup SASL to login (or use NickServ IDENTIFY). One of the more complete SASL instruction pages is Freenode's page [here](https://freenode.net/kb/answer/sasl). Open up that page, find your IRC client and then setup SASL with your chosen username and password!

## Account/Nick Modes

Oragono supports several different modes of operation with respect to accounts and nicknames.

### Traditional / lenient mode

This is the default mode, and makes Oragono's services act similar to Quakenet's Q bot. In this mode, users cannot own or reserve nicknames. In other words, there is no connection between account names and nicknames. Anyone can use any nickname (as long as it's not already in use by another running client). However, accounts are still useful: they can be used to register channels (see below), and some IRCv3-capable clients (with the `account-tag` or `extended-join` capabilities) may be able to take advantage of them.

To enable this mode, set the following configs (this is the default mode):

* `accounts.registration.enabled = true`
* `accounts.authentication-enabled = true`
* `accounts.nick-reservation.enabled = false`

### Nick ownership

This mode makes Oragono's services act like those of a typical IRC network (like Freenode). In this mode, registering an account gives you privileges over the use of that account as a nickname. The server will then help you to enforce control over your nickname(s):

* You can proactively prevent anyone from using your nickname, unless they're already logged into your account
* Alternately, you can give clients a grace period to log into your account, but if they don't and the grace period expires, the server will change their nickname to something else
* Alternately, you can forego any proactive enforcement – but if you decide you want to reclaim your nickname from a squatter, you can `/msg Nickserv ghost stolen_nickname` and they'll be disconnected
* You can associate additional nicknames with your account by changing to it and then issuing `/msg NickServ group`

To enable this mode, set the following configs:

* `accounts.registration.enabled = true`
* `accounts.authentication-enabled = true`
* `accounts.nick-reservation.enabled = true`

The following additional configs may be of interest:

* `accounts.nick-reservation.method = timeout` ; setting `strict` here effectively forces people to use SASL, and some popular clients either do not support SASL, or have bugs in their SASL implementations.
* `accounts.nick-reservation.allow-custom-enforcement = true` ; this allows people to opt into strict enforcement or opt out of enforcement as they wish. For details on how to do this, `/msg NickServ help enforce`.

### SASL-only mode

This mode is comparable to Slack, Mattermost, or similar products intended as internal chat servers for an organization or team. In this mode, clients cannot connect to the server unless they log in with SASL as part of the initial handshake. This allows Oragono to be deployed facing the public Internet, with fine-grained control over who can log in.

In this mode, clients must have a valid account to connect, so they cannot register their own accounts. Accordingly, an operator must do the initial account creation, using the `SAREGISTER` command of NickServ. (For more details, `/msg NickServ help saregister`.) To bootstrap this process, you can make an initial connection from localhost, which is exempt (by default) from the requirement, or temporarily add your own IP to the exemption list. You can also use a more permissive configuration for bootstrapping, then switch to this one once you have your account. Another possibility is permanently exempting an internal network, e.g., `10.0.0.0/8`, that only trusted people can access.

To enable this mode, set the following configs:

* `accounts.registration.enabled = false`
* `accounts.authentication-enabled = true`
* `accounts.require-sasl.enabled = true`
* `accounts.nick-reservation.enabled = true`

Additionally, the following config is recommended:

* `accounts.nick-reservation.method = strict`


## Channel Registration

Once you've registered an account, you can also register channels. If you own a channel, you'l be opped whenever you join it, and the topic/modes will be remembered and re-applied whenever anyone rejoins the channel.

To register a channel, make sure you're joined to it and logged into your account. If both those are true, you can send this command to register your account:

        /CS REGISTER #channelname

For example, `/CS REGISTER #channel` will register the channel `#test` to my account. If you have a registered channel, you can use `/CS OP #channel` to regain ops in it. Right now, the options for a registered channel are pretty sparse, but we'll add more as we go along.

If your friends have registered accounts, you can automatically grant them operator permissions when they join the channel. For more details, see `/CS HELP AMODE`.


## Language

Oragono supports multiple languages! Specifically, once you connect you're able to get server messages in other languages (messages from other users will still be in their original languages, though).

To see which languages are supported, run this command:

        /QUOTE CAP LS 302

In the resulting text, you should see a token that looks something like this:

        draft/languages=11,en,~ro,~tr-TR,~el,~fr-FR,~pl,~pt-BR,~zh-CN,~en-AU,~es,~no

That's the list of languages we support. For the token above, the supported languages are:

- `en`: English
- `en-AU`: Australian English
- `el`: Greek
- `es`: Spanish
- `fr-FR`: French
- `no`: Norwegian
- `pl`: Polish
- `pt-BR`: Brazilian Portugese
- `ro`: Romanian
- `tr-TR`: Turkish
- `zh-CN`: Chinese

To change to a specific language, you can use the `LANGUAGE` command like this:

        /LANGUAGE ro zh-CN

The above will change the server language to Romanian, with a fallback to Chinese. English will always be the final fallback, if there's a line that is not translated. Substitute any of the other language codes in to select other languages, and run `/LANGUAGE en` to get back to standard English.

Our language and translation functionality is very early, so feel free to let us know if there are any troubles with it! If you know another language and you'd like to contribute, we've got a CrowdIn project here: [https://crowdin.com/project/oragono](https://crowdin.com/project/oragono)


-------------------------------------------------------------------------------------------


# Frequently Asked Questions

Some troubleshooting, some general questions about the project! This should help answer any sorta queries you have.


## I have a suggestion

Awesome! We love getting new suggestions for features, ways to improve the server and the tooling around it, everything.

There are two ways to make suggestions, either:

- Submit an issue on our [bug tracker](https://github.com/oragono/oragono/issues).
- Talk to us in the `#oragono` channel on Freenode.


## Why can't I oper?

If you try to oper unsuccessfully, Oragono will disconnect you from the network. Here's some important things to try if you find yourself unable to oper:

1. Have you generated the config-file password blob with `oragono genpasswd`?
2. Have you restarted Oragono to make sure the new password has taken effect?
3. If all else fails, can you get raw protocol output from Oragono for evaluation?

So, first off you'll want to make sure you've stored the password correctly. In the config file, all passwords are bcrypted. Basically, you run `oragono genpasswd`, type your actual password in, and then receive a config file blob back. Put that blob into the config file, and then use the actual password in your IRC client.

After that, try restarting Oragono to see if that improves things. Even if you've already done so or already rehashed, a proper restart can sometimes help things. Make sure your config file is saved before restarting the server.

If both of those have failed, it might be worth getting us to look at the raw lines and see what's up.

If you're familiar with getting this output through your client (e.g. in weechat it's `/server raw`) then you can do so that way, or use [ircdog](https://github.com/goshuirc/ircdog).

Otherwise, in the Oragono config file, you'll want to enable raw line logging by removing `-userinput -useroutput` under the `logging` section. Once you start up your server, connect, fail to oper and get disconnected, you'll see a bunch of input/output lines in Ora's log file. Remove your password from those logs and pass them our way.

## How do I use Let's Encrypt certificates?

Every deployment's gonna be different, but you can use certificates from [Let's Encrypt](https://letsencrypt.org) without too much trouble. Here's some steps that should help get you on the right track:

1. Follow this [guidance](https://letsencrypt.org/getting-started/) from Let's Encrypt to create your certificates.
2. You should now have a set of `pem` files, Mainly, we're interested in your `live/` Let's Encrypt directory (e.g. `/etc/letsencrypt/live/<site>/`).
3. Here are how the config file keys map to LE files:
    - `cert: tls.crt` is `live/<site>/fullchain.pem`
    - ` key: tls.key` is `live/<site>/privkey.pem`
4. You may need to copy the `pem` files to another directory so Oragono can read them, or similarly use a script like [this one](https://github.com/darwin-network/slash/blob/master/etc/bin/install-lecerts) to automagically do something similar.
5. By default, `certbot` will automatically renew your certificates. Oragono will only reread certificates when it is restarted, or during a rehash (e.g., on receiving the `/rehash` command or the `SIGHUP` signal). You can add an executable script to `/etc/letsencrypt/renewal-hooks/post` that can perform the rehash. Here's one example of such a script:

        #!/bin/bash
        pkill -HUP oragono

The main issues you'll run into are going to be permissions issues. This is because by default, certbot will generate certificates that non-root users can't (and probably shouldn't) read. If you run into trouble, look over the script in step **4** and/or make sure you're copying the files to somewhere else, as well as giving them correct permissions with `chown`, `chgrp` and `chmod`.

On other platforms or with alternative ACME tools, you may need to use other steps or the specific files may be named differently.


{% include hrsec.html %}


# Modes

On IRC, you can set modes on users and on channels. Modes are basically extra information that changes how users and channels work.

In this section, we give an overview of the modes Oragono supports.


## User Modes

These are the modes which can be set on you when you're connected.

### +a - Away

If this mode is set, you're marked as 'away'. To set and unset this mode, you use the `/AWAY` command.

### +i - Invisible

If this mode is set, you're marked as 'invisible'. This means that your channels won't be shown when users `/WHOIS` you (except for IRC operators, they can see all the channels you're in).

To set this mode on yourself:

        /mode dan +i

### +o - Operator

If this mode is set, you're marked as an 'IRC Operator'. This means that you're an admin of some sort on the server and have some special powers regular users don't have. To set this mode, you authenticate (oper-up) using the `/OPER` command.

### +R - Registered-Only

If this mode is set, you'll only receive messages from other users if they're logged into an account. If a user who isn't logged-in messages you, you won't see their message.

To set this mode on yourself:

        /mode dan +R

To unset this mode and let anyone speak to you:

        /mode dan -R

### +s - Server Notice Masks

This is a special 'list mode'. If you're an IRC operator, this mode lets you see special server notices that get sent out. See the Server Notice Masks section for more information on this mode.

### +Z - TLS

This mode is automatically set if you're connecting using SSL/TLS. There's no way to set this yourself, and it's automatically set or not set when you connect to the server.


## Channel Modes

These are the modes that can be set on channels when you're an oper!

### +b - Ban

With this channel mode, you can change and see who's banned from the channel. Specifically, you can ban 'masks', or a set of nickname, username and hostname.

Here's an example of banning a user named **bob** from channel #test:

        /MODE #test +b bob!*@*

Let's say that **bob** is connecting from the address `192.168.0.234`. You could also do this to ban him:

        /MODE #test +b *!*@192.168.0.234

Banning **bob** in this way means that nobody from that address can connect.

To remove a ban, you do the same thing with `-b` instead of `+b`.

To view the bans that exist on the channel, you can do this instead:

        /MODE #test b

### +e - Ban-Exempt

With this channel mode, you can change who's allowed to bypass bans. For example, let's say you set these modes on the channel:

        /MODE #test +b *!*@192.168.0.234
        /MODE #test +e bob!*@*

This means that **bob** will always be able to join, even if he's connecting from `192.168.0.234`.

For everything else, this mode acts like the `+b - Ban` mode.

### +i - Invite-Only

If this channel mode is set on a channel, users will only be able to join if someone has `/INVITE`'d them first.

To set a channel to invite-only:

        /MODE #test +i

To unset the mode and let anyone join:

        /MODE #test -i

### +I - Invite-Exempt

With this channel mode, you can change who's allowed to join the channel when the `+i - Invite-Only` mode is enabled.

For example, let's say you set these modes on the channel:

        /MODE #test +i
        /MODE #test +I bob!*@*

This means that **bob** will be able to join even without being `/INVITE`'d.

For everything else, this mode acts like the `+b - Ban` mode.

### +k - Key

This channel mode lets you set a 'key' that other people will need to join your channel. To set a key:

        /MODE #test +k p4ssw0rd

Then, to join users will need to do `/JOIN #test p4ssw0rd`. If they try to join without the key, they will be rejected.

To unset the key:

        /MODE #test -k

### +l - Limit

This mode lets you restrict how many users can join the channel.

Let's say that `#test` currently has 5 users in it, and you run this command:

        /MODE #test +l 6

Only one more user will be able to join the channel. If anyone tries to join the channel when there's already six people on it, they will get rejected.

Just like the `+k - Key` mode, to unset the limit:

        /MODE #test -l

### +m - Moderated

This mode lets you restrict who can speak in the channel. If the `+m` mode is enabled, normal users won't be able to say anything. Users who are Voice, Halfop, Channel-Op, Admin and Founder will be able to talk.

To set this mode:

        /MODE #test +m

To unset this mode (and let everyone speak again):

        /MODE #test -m

### +n - No Outside Messages

This mode is enabled by default, and means that only users who are joined to the channel can send messages to it.

If this mode is unset, users who aren't on your channel can send messages to it. This can be useful with, for example, GitHub or notification bots if you want them to send messages to your channel but don't want them to clutter your channel with by joining and leaving it.

### +R - Registered Only

If this mode is set, only users that have logged into an account will be able to join and speak on the channel. If this is set and a regular, un-logged-in user tries to join, they will be rejected.

To set this mode:

        /MODE #test +R

To unset this mode:

        /MODE #test -R

### +s - Secret

If this mode is set, it means that your channel should be marked as 'secret'. Your channel won't show up in `/LIST` or `/WHOIS`.

To set this mode:

        /MODE #test +s

To unset this mode:

        /MODE #test -s

### +t - Op-Only Topic

This mode is enabled by default, and means that only channel operators can change the channel topic (using the `/TOPIC` command).

If this mode is unset, anyone will be able to change the channel topic.


## Channel Prefixes

Users on a channel can have different permission levels, which are represented by having different characters in front of their nickname. This section explains the prefixes and what each one means.

### +q (~) - Founder

This prefix means that the given user is the founder of the channel. For example, if `~dan` is on a channel it means that **dan** founded the channel. The 'founder' prefix only appears on channels that are registered.

Founders are able to do anything, and have complete administrative control of the channel.

### +a (&) - Admin

This prefix means that the given user is an admin on the channel. For example, if `&tom` is on a channel, then **tom** is an admin on it. The 'admin' prefix only appears on channels that are registered.

Admins can do anything channel operators can do, and they also cannot get kicked by other chanops or admins.

### +o (@) - Channel Operator

This prefix means that the given user is an operator on the channel (chanop, for short). For example, if `@ruby` is on a channel, then **ruby** is an op.

Chanops are the regular type of channel moderators. They can set the topic, change modes, ban/kick users, etc.

### +h (%) - Halfop

This prefix means that the given user is a halfop on the channel (half-operator). For example, if `%twi` is on a channel, then **twi** is a halfop.

Halfops can do some of what channel operators can do, and can't do other things. They can help moderate a channel.

### +v (+) - Voice

This prefix means that the given user is 'voiced' on the channel. For example, if `+faust` is on a channel, then **faust** is voiced on that channel.

Voiced users can speak when the channel has `+m - Moderated` mode enabled. They get no other special privs or any moderation abilities.


{% include hrsec.html %}


# Commands

The best place to look for command help is on a running copy or Oragono itself!

To see the integrated command help, simply spin up a copy of Oragono and then run this command:

        /HELPOP <command>

If that doesn't work, you may need to run this instead:

        /QUOTE HELP <command>

We may add some additional notes here for specific commands down the line, but right now the in-server docs are the best ones to consult.


{% include hrsec.html %}


# Integrating with other software

Oragono should interoperate with most IRC-based software, including bots. If you have problems getting your preferred software to work with Oragono, feel free to report it to us. If the root cause is a bug in Oragono, we'll fix it.

One exception is services frameworks like [Anope](https://github.com/anope/anope) or [Atheme](https://github.com/atheme/atheme); we have our own services implementations built directly into the server, and since we don't support federation, there's no place to plug in an alternative implementation.

If you're looking for a bot that supports modern IRCv3 features, check out [bitbot](https://github.com/jesopo/bitbot/)!

## Hybrid Open Proxy Monitor (HOPM)

[hopm](https://github.com/ircd-hybrid/hopm) can be used to monitor your server for connections from open proxies, then automatically ban them. To configure hopm to work with oragono, add operator blocks like this to your oragono config file, which grant hopm the necessary privileges:

        # operator classes
        oper-classes:
            # hopm
            "hopm":
                # title shown in WHOIS
                title: Proxy Monitor

                # capability names
                capabilities:
                - "oper:local_kill"
                - "oper:local_ban"
                - "oper:local_unban"
                - "nofakelag"

        # ircd operators
        opers:
            # operator named 'hopm'
            hopm:
                # which capabilities this oper has access to
                class: "hopm"

                # custom hostname
                vhost: "proxymonitor.hopm"

                # modes are the modes to auto-set upon opering-up
                modes: +is c

                # password to login with /OPER command
                # generated using  "oragono genpasswd"
                password: "$2a$04$JmsYDY6kX3/wwyK3ao0L7.aGJEto0Xm4DyL6/6zOmCpzeweIb8kdO"

Then configure hopm like this:

        /* oragono */
        connregex = ".+-.+CONNECT.+-.+ Client Connected \\[([^ ]+)\\] \\[u:([^ ]+)\\] \\[h:([^ ]+)\\] \\[ip:([^ ]+)\\] .+";

        /* A DLINE example for oragono */
        kline = "DLINE ANDKILL 2h %i :Open proxy found on your host.";

## ZNC

Versions of ZNC prior to 1.7 have a [bug](https://github.com/znc/znc/issues/1212) in their SASL implementation that renders them incompatible with Oragono. However, you should be able to authenticate from ZNC using its [NickServ](https://wiki.znc.in/Nickserv) module.


{% include hrsec.html %}


# Acknowledgements

Always, thanks to Jeremy Latt for creating Ergonomadic. Thanks for Edmund Huber for maintaining Ergonomadic and providing useful help while transitioning.

Thanks to Euan Kemp (euank) for the contributions and help with this, along with other projects, and to James Mills, Vegax and Sean Enck for various other help and contributions on the server.

And a massive thanks to Shivaram Lingamneni (slingamn) for being an awesome co-maintainer of Oragono! You really convinced me to step up with this and take it forward in a big way, and I'm grateful for that.

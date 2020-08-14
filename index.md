---
title: Home
layout: default
meta-description: Oragono is a modern IRC server written in Go.
---
{% include header.html %}

Oragono is a modern IRC server written in Go. Its core design principles are:

* Being simple to set up and use
* Combining the features of an ircd, a services framework, and a bouncer (integrated account management, history storage, and bouncer functionality)
* Bleeding-edge [IRCv3](https://ircv3.net) support, suitable for use as an IRCv3 reference implementation
* Highly customizable via a rehashable (i.e., reloadable at runtime) YAML config

Other interesting features of Oragono include:

* Advanced internationalization support, including UTF-8 nicknames and channel names
* Support for running as a Tor onion service ("hidden service")

<div class="center">
[https://github.com/oragono/oragono]

chat with us at [#oragono on Freenode](ircs://irc.freenode.net:6697/#oragono)

play with a running instance at <a href="https://testnet.oragono.io">https://testnet.oragono.io</a>
</div>

{% include hr.html %}

<pre>
<strong>~=~ Installing and Using ~=~</strong>

To install Oragono, download the latest release here:
 <a href="https://github.com/oragono/oragono/releases">https://github.com/oragono/oragono/releases</a>

Extract it into your favourite folder, along with the example config file.

Copy the example config file to  ircd.yaml  with a command like:

    <span class="term">$</span> cp oragono.yaml ircd.yaml

Modify the config file as you like.

To generate passwords for opers and connect passwords, you can use this command:

    <span class="term">$</span> oragono genpasswd

Run these commands in order -- these will setup each section of the server:

    <span class="term">$</span> oragono initdb
    <span class="term">$</span> oragono mkcerts
    <span class="term">$</span> oragono run

And you should be running Oragono!


<strong>~=~ Updating ~=~</strong>

If you're updating from a previous version of Oragono, checkout the CHANGELOG for a shortlist
of important changes you'll want to take a look at. The change log details config changes,
fixes, new features and anything else you'll want to be aware of!

These commands will backup and update your database, if that's been updated:

    <span class="term">$</span> cp ircd.db ircd.db.bak
    <span class="term">$</span> oragono upgradedb

After this, you should be fine to launch and use Oragono!
</pre>

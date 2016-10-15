---
title: Home
layout: default
meta-description: Oragono is a modern, experimental IRC server written in Go.
---
{% include header.html %}

<pre>
Oragono is a modern, experimental IRC server written in Go. It's designed to be simple to setup
and use, and to provide the majority of features that IRC users expect today.

It includes features such as UTF-8 nicks and channel names, client accounts and SASL, and other
assorted IRCv3 support.

-----------------------------------------------------------------------------------------------


=== Installing ===

To install Oragono, download the latest release here:
<a href="https://github.com/DanielOaks/oragono/releases">https://github.com/DanielOaks/oragono/releases</a>

Extract it into your favourite folder, along with the example config file.

Copy the example config file to  ircd.yaml  with a command like:

    <span class="term">$</span> cp oragono.yaml ircd.yaml

Modify the config file as you like.

Run these commands in order -- these will setup each section of the server:

    <span class="term">$</span> oragono initdb
    <span class="term">$</span> oragono mkcerts
    <span class="term">$</span> oragono run

And you should be running Oragono!
</pre>

---
title: Home
layout: default
meta-description: Oragono is a modern, experimental IRC server written in Go.
---
{% include header.html %}

<pre>
Oragono is a modern, experimental IRC server written in Go. It's designed to be simple to setup
and use and to provide the majority of features that IRC users expect today.

It includes features such as UTF-8 nicks and channel names, client accounts with SASL, and
other assorted IRCv3 support.

                             <a href="https://github.com/DanielOaks/oragono">https://github.com/DanielOaks/oragono</a>

                                 chat at <a href="ircs://irc.freenode.net:6697/#oragono">#oragono on Freenode</a>

{% include hr.html %}


<strong>~=~ Installing and Using ~=~</strong>

To install Oragono, download the latest release here:
 <a href="https://github.com/DanielOaks/oragono/releases">https://github.com/DanielOaks/oragono/releases</a>

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

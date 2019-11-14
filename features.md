---
title: Features
layout: default
meta-description: Things you can do with the Oragono IRC server.
---
{% include header.html %}

<pre>
Here's a few things you can do with Oragono: 


<strong>~=~ Register an account ~=~</strong>

1. Make sure "accounts.registration" is enabled and setup correctly in your config.

2. Connect to your server.

3. Register your current nickname as an account:

    - <strong>Passphrase</strong>:    /NS REGISTER mypasswordhere
    - <strong>CertFP (TLS)</strong>:  /NS REGISTER *


<strong>~=~ Authenticate for your account ~=~</strong>

1. Make sure "accounts.authentication-enabled" is true in your config.

2. Setup SASL in your client (using instructions such as <a href="https://freenode.net/kb/answer/sasl">these</a>).

3. Done!


<strong>~=~ Register a channel ~=~</strong>

1. Make sure "channels.registration" is enabled and setup correctly in your config.

2. Make sure you're logged into your account.

3. Make sure you're an op in the given channel.

4. Send a message to ChanServ with this command:

    /CS REGISTER #channel

5. Done! Any topic updates will be remembered, and you'll be opped upon joining.

</pre>

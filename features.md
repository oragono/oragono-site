---
title: Downloads
layout: default
meta-description: Download links for the Oragono IRC server.
---
{% include header.html %}

<pre>
Here's a few things you can do with Oragono: 


<strong>~=~ Register an account ~=~</strong>

1. Make sure "accounts.registration" is enabled and setup correctly in your config.

2. Connect to your server.

3. Register your account using the REG command:

    - <strong>Passphrase</strong>:    /QUOTE REG CREATE dan * passphrase :mypasswordhere
    - <strong>CertFP (TLS)</strong>:  /QUOTE REG CREATE dan * certfp *


<strong>~=~ Authenticate for your account ~=~</strong>

1. Setup SASL in your client.

2. Done!


<strong>~=~ Register a channel ~=~</strong>

1. Make sure "channels.registration" is enabled and setup correctly in your config.

2. Make sure you're logged into your account.

3. Make sure you're an op in the given channel.

4. Send a message to ChanServ with this command:

    /CS REGISTER #channel

5. Done! Any topic updates will be remembered, and you'll be opped upon joining.

</pre>
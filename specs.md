---
title: Specs
layout: default
meta-description: Specification links for the Oragono IRC server.
---
{% include header.html %}

<pre>
Oragono roughly draws from these specifications:

 - Modern IRC: <a href="http://modern.ircdocs.horse/">http://modern.ircdocs.horse/</a>

 - IRCv3: <a href="http://ircv3.net/">http://ircv3.net/</a>


Oragono also implements these specifications:

 - <a href="/maxline-2.html">oragono.io/maxline-2</a> - Longer IRC lines.

 - <a href="/bnc.html">oragono.io/bnc</a> - Allowing multiple clients to share a nickname.

 - <a href="/nope.html">oragono.io/nope</a> - A way of detecting incorrect client implementations.


Oragono also implements these specifications proposed to IRCv3:

 - <a href="https://github.com/DanielOaks/ircv3-specifications/blob/register-and-verify/extensions/acc-core.md">acc-core</a> - Account registration.

 - <a href="https://gist.github.com/DanielOaks/8126122f74b26012a3de37db80e4e0c6">language</a> - Server language negotiation.

 - <a href="https://github.com/SaberUK/ircv3-specifications/blob/rename/extensions/rename.md">rename</a>   - Channel renaming.

 - <a href="https://github.com/DanielOaks/ircv3-specifications/blob/master+resume/extensions/resume.md">resume</a>   - Connection resumption.

 - <a href="https://github.com/DanielOaks/ircv3-specifications/blob/master%2Brfc7700/documentation/rfc7613.md">rfc7613</a>  - Unicode nick and channel names.


And these specifications I've written up based on existing commands and behaviour:

 - <a href="https://gist.github.com/DanielOaks/420d14efbbcda93a7d7e1dc00bf7dc25">Roleplay commands</a>


For other specifications you should consider looking at, see this page:
<a href="http://ircdocs.horse/specs/">http://ircdocs.horse/specs/</a>

{% include hr.html %}


<strong>~=~ IRCv3 Support ~=~</strong>

The following stable extensions are supported:

 - <a href="http://ircv3.net/specs/core/capability-negotiation-3.1.html">Capability Negotiation 3.1</a>
 - <a href="http://ircv3.net/specs/core/capability-negotiation-3.2.html">Capability Negotiation 3.2</a>
 - <a href="http://ircv3.net/specs/extensions/cap-notify-3.2.html">cap-notify</a>

 - <a href="http://ircv3.net/specs/extensions/account-notify-3.1.html">account-notify</a>
 - <a href="http://ircv3.net/specs/extensions/account-tag-3.2.html">account-tag</a>
 - <a href="http://ircv3.net/specs/extensions/away-notify-3.1.html">away-notify</a>
 - <a href="https://ircv3.net/specs/extensions/batch-3.2.html">batch</a>
 - <a href="http://ircv3.net/specs/extensions/chghost-3.2.html">chghost</a>
 - <a href="http://ircv3.net/specs/extensions/echo-message-3.2.html">echo-message</a>
 - <a href="http://ircv3.net/specs/extensions/extended-join-3.1.html">extended-join</a>
 - <a href="http://ircv3.net/specs/extensions/invite-notify-3.2.html">invite-notify</a>
 - <a href="http://ircv3.net/specs/core/monitor-3.2.html">monitor</a>
 - <a href="http://ircv3.net/specs/extensions/message-ids.html">message-ids</a>
 - <a href="https://ircv3.net/specs/extensions/message-tags">message-tags</a>
 - <a href="http://ircv3.net/specs/extensions/multi-prefix-3.1.html">multi-prefix</a>
 - <a href="http://ircv3.net/specs/extensions/sasl-3.1.html">sasl 3.1</a>
 - <a href="http://ircv3.net/specs/extensions/sasl-3.2.html">sasl 3.2</a>
 - <a href="http://ircv3.net/specs/extensions/server-time-3.2.html">server-time</a>
 - <a href="http://ircv3.net/specs/extensions/sts.html">sts</a>
 - <a href="http://ircv3.net/specs/extensions/userhost-in-names-3.2.html">userhost-in-names</a>

The following draft extensions are supported:

 - <a href="https://ircv3.net/specs/extensions/labeled-response.html">labeled-responses</a>


{% include hr.html %}

Generally, I try to keep Oragono close to how other IRC software behaves.

However, if there are any issues or there's a specific case you'd like to report,
please visit the issue tracker here: <a href="https://github.com/oragono/oragono/issues">https://github.com/oragono/oragono/issues</a>
</pre>

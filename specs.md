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


Oragono also implements these proposed specifications:

 - <a href="https://github.com/DanielOaks/ircv3-specifications/blob/register-and-verify/extensions/reg-core-3.3.md">reg-core</a> - Account registration.

 - <a href="https://github.com/DanielOaks/ircv3-specifications/blob/master%2Brfc7700/documentation/rfc7700.md">rfc7700</a>  - Unicode nick and channel names.


For other specifications you should consider looking at, see this page:
<a href="http://ircdocs.horse/specs/">http://ircdocs.horse/specs/</a>

-----------------------------------------------------------------------------------------------


=== IRCv3 Support ===

The following 3.1 extensions are supported:

 - <a href="http://ircv3.net/specs/core/capability-negotiation-3.1.html">Capability Negotiation</a>
 - <a href="http://ircv3.net/specs/extensions/sasl-3.1.html">account-notify</a>
 - <a href="http://ircv3.net/specs/extensions/away-notify-3.1.html">away-notify</a>
 - <a href="http://ircv3.net/specs/extensions/extended-join-3.1.html">extended-join</a>
 - <a href="http://ircv3.net/specs/extensions/multi-prefix-3.1.html">multi-prefix</a>
 - <a href="http://ircv3.net/specs/extensions/sasl-3.1.html">sasl</a>

The following 3.2 extensions are supported:

 - <a href="http://ircv3.net/specs/extensions/account-tag-3.2.html">account-tag</a>
 - <a href="http://ircv3.net/specs/extensions/server-time-3.2.html">server-time</a>
 - <a href="http://ircv3.net/specs/extensions/userhost-in-names-3.2.html">userhost-in-names</a>

The following 3.3 draft extensions are supported:

- <a href="http://ircv3.net/specs/core/message-tags-3.3.html">message-tags</a> as draft/message-tags


-----------------------------------------------------------------------------------------------

Generally, I try to keep Oragono roughly close to how other IRC software behaves.

However, if there are any issues or there's a specific case you'd like to report,
please visit the issue tracker here: <a href="https://github.com/DanielOaks/oragono/issues">https://github.com/DanielOaks/oragono/issues</a>
</pre>
XMPP Echo Bot
=============

Do you know that situation, you really really need an XMPP echo bot, but you don’t have access to high-level tools like `Python <https://github.com/horazont/aioxmpp>`_ to write one? All you have is `openssl`, `bash`, `dig`, `stdbuf` and `sed`? Then this tool is for you.

This is an XMPP echo bot written in (mostly) sed. Bash is used to do the pre-authentication setup (look up DNS records, establish TLS via ``openssl s_client``). sed processes the XML stream and handles all interaction with the server on the XMPP level. Yes, this kinda parses XML in sed.

Tricks and shortcuts
--------------------

* We use ``tr`` to convert ``>`` to ``\n`` -- since sed is line (or NUL) based, there’s not really another way to parse XMPP XML (which generally never contains newlines) with sed.
* TLS is handled outside of sed for similar reasons. And to keep my sanity (some people might question whether I still have any bit of sanity left).
* Likewise, SRV lookup and composition of the authentication data is entirely handled in bash. This also means that only PLAIN SASL authentication is supported -- SCRAM requires a level of interactivity which would be extremely hard to achieve in sed (not impossible though; we would "just" have to implement base64 and sha1-hmac in sed).

Usage
-----

::

    ./echoz.sh user@domain password
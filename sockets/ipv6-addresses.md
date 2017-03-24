# IPv6 addresses {#ipv6-addresses}

OpenFL 9.0.115.0 and later support IPv6 (Internet Protocol version 6). IPv6 is a version of Internet Protocol that supports 128-bit addresses (an improvement on the earlier IPv4 protocol that supports 32-bit addresses). You might need to activate IPv6 on your networking interfaces. For more information, see the Help for the operating system hosting the data.

If IPv6 is supported on the hosting system, you can specify numeric IPv6 literal addresses in URLs enclosed in brackets ([]), as in the following:

[2001:db8:ccc3:ffff:0:444d:555e:666f]

OpenFL returns literal IPv6 values, according to the following rules:

*   OpenFL returns the long form of the string for IPv6 addresses.
*   The IP value has no double-colon abbreviations.
*   Hexadecimal digits are lowercase only.
*   IPv6 addresses are enclosed in square brackets ([]).
*   Each address quartet is output as 0 to 4 hexadecimal digits, with the leading zeros omitted.
*   An address quartet of all zeros is output as a single zero (not a double colon) except as noted in the following list of exceptions.

The IPv6 values that OpenFL returns have the following exceptions:

*   An unspecified IPv6 address (all zeros) is output as [::].
*   The loopback or localhost IPv6 address is output as [::1].
*   IPv4 mapped (converted to IPv6) addresses are output as [::ffff:a.b.c.d], where a.b.c.d is a typical IPv4 dotted- decimal value.
*   IPv4 compatible addresses are output as [::a.b.c.d], where a.b.c.d is a typical IPv4 dotted-decimal value.
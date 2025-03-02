.. currentmodule:: netaddr

--------------------
NEXT_NETADDR_VERSION
--------------------

Date: not released yet

Added:

* Add CLI tool subcommand to display :ref:`cli-network-info`.

Changed:

* Support running :ref:`interactive-shell` without IPython installed.

--------------
Release: 1.1.0
--------------

Date: 2024-02-15

Added:

* Add the required Python version to the package metadata (GH :issue:`365`).
* Add :func:`expand_partial_ipv4_address` to the public API.

Fixed:

* Fix ``IPNetwork(...) in IPRange(...)`` false negatives (GH :issue:`157`).
* Fix a few :class:`IPNetwork` slicing edge cases (GH :issue:`214`).
* Fix support for partial IP addresses accidentally left in :class:`IPNetwork` in 1.0.0.

  When I removed the ``implicit_prefix`` switch I missed the fact that there was some
  partial IPv4 address expansion triggered unconditionally.

  If you need the old behavior use :func:`expand_partial_ipv4_address`.

  Related GH issue: :issue:`110`.
* Fixed an incorrect license classifier in the package metadata.

--------------
Release: 1.0.0
--------------

Date: 2024-02-10

Removed:

* Drop support for Python versions lower than 3.7.
* Remove the flag shorthands: ``N``, ``P`` and ``Z``. Use :data:`NOHOST`, :data:`INET_PTON`
  and :data:`ZEROFILL` instead.
* Remove abbreviated CIDR format support in :class:`IPNetwork` (``implicit_prefix=True``),
  use :func:`cidr_abbrev_to_verbose` if you need this behavior.
* Remove the ``IPAddress.is_private`` method.

  There are more precise replacements for subset of the addresses that used to handled by ``is_private``:

  * :meth:`IPAddress.is_link_local`
  * :meth:`IPAddress.is_ipv4_private_use`
  * :meth:`IPAddress.is_ipv6_unique_local`
  * :meth:`IPAddress.is_global`

  The following address blocks used to be handled by ``is_private`` have no dedicated
  convenience methods and you'll have to handle them manually or request a method
  addition:

  * ``100.64.0.0/10`` – Shared Address Space
  * ``192.0.0.0/24`` – IETF Protocol Assignments (watch out – there are exceptions in there)
  * ``198.18.0.0/15`` – Benchmarking
  * ``239.0.0.0``-``239.255.255.255`` – ``240.0.0.0/4`` is Reserved, ``239.0.0.0/8`` – unclear

Changed:

* Stop accepting leading zeros when parsing IPv4 addresses in :data:`INET_PTON` mode
  (it's been allowed on some platforms).

  If you need to allow and discard leading zeros use the :data:`ZEROFILL` flag.

  This change will affect implicit conversions from ``str`` in all relevant contexts. If you need
  to control the IPv4 parsing mode construct :class:`IPAddress` objects explicitly.
* Stop parsing IPv4 addresses permissively (``inet_aton()``-like) by default.

  :data:`INET_PTON` is the default mode.

  If you need to be permissive and parse using ``inet_aton()`` semantics use the
  :data:`INET_ATON` flag.

  This change will affect implicit conversions from ``str`` in all relevant contexts. If you
  need to control the IPv4 parsing mode construct :class:`IPAddress` objects explicitly.
* Apply the two changes above to :func:`valid_ipv4` as well.
* Update the address databases to the 2024-02-10 versions.

Fixed:

* Return ``False`` instead of raising :exc:`AddrFormatError` when an empty string is passed
  to :func:`valid_ipv4` or :func:`valid_ipv6`.
* Fix handling of ``dialect`` provided to :class:`EUI` during copy-construction.

---------------
Release: 0.10.1
---------------

Date: 2024-01-02

Fixed:

* Get rid of some warnings

Deprecated:

* Deprecate the flag shorthands: ``N``, ``P`` and ``Z``. Use :data:`NOHOST`, :data:`INET_PTON`
  and :data:`ZEROFILL` instead.
* Deprecate importing objects from ``netaddr`` subpackages. Only importing things from the
  top-level ``netaddr`` namespace is supported, everything else is considered private.

  This has already been the case but we can use a reminder.
* Deprecate permissive-by-default IPv4 parsing in :func:`valid_ipv4`. The ``inet_pton()``
  semantics (with leading zeros always disallowed) will become the default. Use :data:`INET_ATON`
  and/or :data:`ZEROFILL` flags if you need the legacy behavior.

---------------
Release: 0.10.0
---------------

Date: 2023-12-31

Added:

* Add an :data:`INET_ATON` flag to explicitly request ``inet_aton()`` IPv4 parsing semantics
  from :class:`IPAddress`.
* Add an :meth:`IPAddress.is_ipv4_private_use` convenience method.
* Add an :meth:`IPAddress.is_global` convenience method to allow determining if an address is
  considered globally reachable.
* Add an :meth:`IPAddress.is_ipv6_unique_local` convenience method.

Fixed:

* Improve Python 3.13 compatibility, thank you John Eckersberg.

Deprecated:

* Deprecate Python 3.7 support.
* Deprecate abbreviated CIDR format support in :class:`IPNetwork`
  (``implicit_prefix=True``).
* Deprecate accepting leading zeros when parsing IPv4 addresses in :data:`INET_PTON` mode
  (it's been allowed on some platforms).

  If you need to allow and discard leading zeros use the :data:`ZEROFILL` flag.

  This change will affect implicit conversions from ``str`` in all relevant contexts. If you need
  to control the IPv4 parsing mode construct :class:`IPAddress` objects explicitly.
* Deprecate parsing IPv4 addresses permissively (``inet_aton()``-like) by default.

  :data:`INET_PTON` will become the default mode.

  If you need to be permissive and parse using ``inet_aton()`` semantics use the
  :data:`INET_ATON` flag.

  This change will affect implicit conversions from ``str`` in all relevant contexts. If you need
  to control the IPv4 parsing mode construct :class:`IPAddress` objects explicitly.
* Deprecate the ``IPAddress.is_private`` method. Migration instructions in the
  documentation.

Other:

* Raise an exception if invalid flags are passed to ``IPAddress``, ``IPNetwork`` or ``IPRange``.
* Improve the documentation substantially.
* Update the DB files to the latest versions (2023-12-23).

--------------
Release: 0.9.0
--------------

Date: 2023-09-18

Added:

* Add hash capabilities to OUI (#225, amitmi704)

Fixed:

* **Backwards incompatible:** Handle RFC 6164 IPv6 addresses (don't reserve first IP
  address in point-to-point subnets) ($267, Damien Claisse)
* **Technically backwards incompatible:** Fix for is_loopback behaviour – consider
  ``IPNetwork('::1/128')`` to be loopback (#222, #223, niels)
* Include tutorials in source distributions (#215, Louis Sautier)
* Fix a documentation typo (#242, Wouter)
* Fix print syntax in the documentation to be Python 3 compatible (#221, François Magimel)
* Fix the Sphinx syntax in the documentation (#220, François Magimel)

Other:

* Update the databases (#266, Jakub Stasiak)
* Deprecate Python 3.6 (#263, Jakub Stasiak)
* Eliminate unnecessary evals (#228, KOLANICH)

--------------
Release: 0.8.0
--------------
Date: 3 Jul 2020

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.20
^^^^^^^^^^^^^^^^^^^^

* Fixed weak reference support in classes with ``__slots__``
* Added ``__bytes__`` to ``IPAddress`` for intuitive usage, thanks to Michael Belousov.
* Added ``format()`` function to EUI, thanks to Omer Anson.
* Added ``IPNetwork.netmask`` property setter, thanks to Naveen Nathan.
* Added support for IABs in the ``40:D8:55`` OUI, thanks to Brian Maissy.
* Drastically optimized ``spanning_cidr()``, thanks to Brian Maissy.
* Fixed ``"x.x.x.x/x" in IPNetwork`` tests, thanks to xpac1985.
* Added support for passing iterables of ``IPRange`` to ``IPSet`` and ``cidr_merge()``, based
  on a patch by Henry Stern.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 205: https://github.com/netaddr/netaddr/issues/205
    - N log N complexity instead of linear

FIXED Issue 171: https://github.com/netaddr/netaddr/issues/171
    - Efficiently creating a large IPSet from a list of IPRanges?

FIXED Issue 161: https://github.com/netaddr/netaddr/issues/161
    - Weak reference support

^^^^^^^^^^^
Miscellanea
^^^^^^^^^^^

- The next release (0.9.0) will contain a backwards incompatible change in IPNetwork's behaviour.
  It's connected to handling of RFC 6164 IPv6 addresses (/127 and /128): IPNetwork.broadcast will
  return None for those and first addresses in the networks will no longer be excluded when
  iterating IPNetwork and IPNetwork.iter_hosts(). See https://github.com/netaddr/netaddr/pull/168
  (or temporarily reverted commit 2984c0a40a70 in this repository) to see the actual patch.

---------------
Release: 0.7.20
---------------
Date: 19 Jun 2020

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.19
^^^^^^^^^^^^^^^^^^^^

* Fixed returning from an iterator on Python 3.7+, by Sergey Kozlov.
* Fixed Python 3.8 SyntaxWarning on using `is not` with a string literal, by Stefan Nordhausen.
* Fixed DeprecationWarnings by using raw strings for escape characters used in regexes, by Sean McGinnis.
* Improved IPGlob documentation, by obkmeta.
* Fixed exception creation in corner cases by explicitly passing error message params as tuples, by
  Matthias Urlichs.
* Stopped manually replacing shebang of an included script.
* Stopped using __file__ in all code that's expected to run in environments that don't support it
  (like PyOxidizer-produced binaries).
* Updated all databases included in the package.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 182: https://github.com/netaddr/netaddr/issues/182
    - test_ip_splitter_remove_prefix_larger_than_input_range fails with python 3.7

FIXED Issue 206: https://github.com/netaddr/netaddr/issues/206
    - ""is not" with a literal." SyntaxWarning

FIXED Issue 198: https://github.com/netaddr/netaddr/issues/198
    - oui databases are outdated

FIXED Issue 188: https://github.com/netaddr/netaddr/issues/188
    - Avoid use of __file__

^^^^^^^^^^^
Miscellanea
^^^^^^^^^^^

- Python 2 versions older than 2.7 and Python 3 versions older than 3.5 should be considered
  unsupported. No incompatible code has been introduced to the best of our knowledge but
  there's no CI infrastructure in place to verify this and if there are any issues with
  those versions they won't be fixed.
- Consequently, Python 2.7 and 3.5 support should be considered deprecated as 2.7 has
  reached its end-of-life already and 3.5 will hit it soon.
- A CI setup has been introduced which allows us to test on a variety of Python versions
  on all Mac, Linux and Windows.

---------------
Release: 0.7.19
---------------
Date: 11 Jan 2017

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.18
^^^^^^^^^^^^^^^^^^^^

* added a new SubnetSplitter class for those looking to divide up subnets.
  Thanks alanwill and RyPeck and those on (Stack Overflow discussion).

* removed bundled pytest dependency code for "python setup.py test".

* setup.py now uses setuptools only (no more distutils) and setup_egg.py removed.

* cleaned up INSTALL docs so they accurately reflect current Python packaging.

* fixed broken parsing, generating and reading of IEEE index files when switching
  between Python 2.x and 3.x.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 133: https://github.com/drkjam/netaddr/issues/133
    - Splitting a single network into multiple prefixed networks

FIXED Issue 129: https://github.com/drkjam/netaddr/issues/129
    - fix IPAddress().netmask_bits to return 0 for 0.0.0.0 and [::] addresses

FIXED Issue 117: https://github.com/drkjam/netaddr/issues/117
    - (python setup.py test) failing with python3 >= 3.5

FIXED Issue 137: https://github.com/drkjam/netaddr/issues/137
    - API reference is broken on ReadTheDocs

FIXED Issue 143: https://github.com/drkjam/netaddr/issues/143
    - Please refresh the bundled IANA and IEEE databases

^^^^^^^^^^^
Miscellanea
^^^^^^^^^^^

- Goodbye to NYSE Euronext (good times), hello Intercontinental Exchange ...

---------------
Release: 0.7.18
---------------
Date: 4 Sep 2015

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.17
^^^^^^^^^^^^^^^^^^^^

* cidr_merge() algorithm is now O(n) and much faster.
  Thanks to Anand Buddhdev (aabdnn) and Stefan Nordhausen (snordhausen).

* nmap target specification now fully supported including IPv4 CIDR
  prefixes and IPv6 addresses.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 100: https://github.com/drkjam/netaddr/issues/100
    - nmap.py - CIDR targets

FIXED Issue 112: https://github.com/drkjam/netaddr/issues/112
    - Observation: netaddr slower under pypy

---------------
Release: 0.7.17
---------------
Date: 31 Aug 2015

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.16
^^^^^^^^^^^^^^^^^^^^

* Fixed a regression with valid_mac due to shadow import in the
  netaddr module.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 114: https://github.com/drkjam/netaddr/issues/114
    - netaddr.valid_mac('00-B0-D0-86-BB-F7')==False for 0.7.16 but True for 0.7.15

---------------
Release: 0.7.16
---------------
Date: 30 Aug 2015

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.15
^^^^^^^^^^^^^^^^^^^^

* IPv4 networks with /31 and /32 netmasks are now treated according to
    RFC 3021. Thanks to kalombos and braaen.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 109: https://github.com/drkjam/netaddr/issues/109
    - Identify registry of global IPv6 unicast allocations

FIXED Issue 108: https://github.com/drkjam/netaddr/issues/108
    - One part of docs unclear?

FIXED Issue 106: https://github.com/drkjam/netaddr/issues/106
    - Eui64 Updated (pull request for Issue 105)

FIXED Issue 105: https://github.com/drkjam/netaddr/issues/105
    - Support dialects for EUI-64 addresses

FIXED Issue 102: https://github.com/drkjam/netaddr/issues/102
    - 0.7.15 tarball is missing tests.

FIXED Issue 96: https://github.com/drkjam/netaddr/issues/96
    - Wrong hosts and broadcasts for /31 and /32 networks.

---------------
Release: 0.7.15
---------------
Date: 29 Jun 2015

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.14
^^^^^^^^^^^^^^^^^^^^

* Fix slowness in IPSet.__contains__. Thanks to novas0x2a for noticing.

* Normalize IPNetworks when they are added to an IPSet

* Converted test suite to py.test

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 98: https://github.com/drkjam/netaddr/issues/98
    - Convert test suite to py.test

FIXED Issue 94: https://github.com/drkjam/netaddr/issues/94
    - IPSet.__contains__ is about 40 times slower than the equivalent IPRange

FIXED Issue 95: https://github.com/drkjam/netaddr/issues/95
    - Inconsistent Address Handling in IPSet

---------------
Release: 0.7.14
---------------
Date: 31st Mar 2015

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.13
^^^^^^^^^^^^^^^^^^^^

* Fix weird build breakage in 0.7.13 (wrong Python path, incorrect OUI DB).

* EUI, OUI, and IAB objects can now be compared with strings. You can do
    my_mac = EUI("11:22:33:44:55:66")
    my_mac == "11:22:33:44:55:66"
    and Python will return True on the "==" operator.

* Implement the "!=" operator for OUI and IAB under Python2. It was already
    working under Python3.

* 64 bit EUIs could only be created from strings with "-" as a separator.
    Now, ":" and no separator are supported, which already worked for 48 bit EUIs.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 80: https://github.com/drkjam/netaddr/pull/80
    - Compare L2 addresses with their representations

FIXED Issue 81: https://github.com/drkjam/netaddr/issues/81
    - OUI database tests fail in 0.7.13

FIXED Issue 84: https://github.com/drkjam/netaddr/issues/84
    - Incorrect python executable path in netaddr-0.7.13-py2.py3-none-any.whl

FIXED Issue 87: https://github.com/drkjam/netaddr/pull/87
    - Handle eui64 addresses with colon as a delimiter and without delimiter.

---------------
Release: 0.7.13
---------------
Date: 31st Dec 2014

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.12
^^^^^^^^^^^^^^^^^^^^

* IPAddress objects can now be added to/subtracted from each other

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 73: https://github.com/drkjam/netaddr/issues/73
    - Adding IP Addresses

FIXED Issue 74: https://github.com/drkjam/netaddr/issues/74
    - compute static global ipv6 addr from the net prefix and mac address

FIXED Issue 75: https://github.com/drkjam/netaddr/issues/75
    - add classifiers for python 3.3 and 3.4 support

---------------
Release: 0.7.12
---------------
Date: 6th Jul 2014

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.11
^^^^^^^^^^^^^^^^^^^^

* Added method IPSet.iter_ipranges().

* bool(IPSet()) works now for large IPSets, e.g. IPSet(['2405:8100::/32']).

* IPNetwork.iter_hosts now skips the subnet-router anycast address for IPv6.

* Removed function fbsocket.inet_aton because it is unused and unnecessary

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 69: https://github.com/drkjam/netaddr/issues/69
    - Add __nonzero__ method to IPSet

FIXED Pull Request 68: https://github.com/drkjam/netaddr/pull/68
    - Fixed a bug related to allowing ::0 during iter_hosts for v6

FIXED Issue 67: https://github.com/drkjam/netaddr/issues/67
    - Remove function fbsocket.inet_aton

FIXED Pull Request 66: https://github.com/drkjam/netaddr/pull/66
    - Added Function to create list of IPRange for non-contiguous IPSet

---------------
Release: 0.7.11
---------------
Date: 19th Mar 2014

^^^^^^^^^^^^^^^^^^^^
Changes since 0.7.10
^^^^^^^^^^^^^^^^^^^^

* Performance of IPSet increased dramatically, implemented by
    Stefan Nordhausen and Martijn van Oosterhout. As a side effect,
    IPSet(IPNetwork("10.0.0.0/8")) is now as fast as you'd expect.

* Various performance improvements all over the place.

* netaddr is now hosted on PyPI and can be installed via pip.

* Doing  "10.0.0.42" in IPNetwork("10.0.0.0/24")  works now.

* IPSet has two new methods: iscontiguous() and iprange(), thanks to Louis des Landes.

* Re-added the IPAddress.netmask_bits() method that was accidentally removed.

* Networks 128.0.0.0/16, 191.255.0.0/16, and 223.255.255.0/24 are not marked as
    reserved IPv4 addresses any more. Thanks to marnickv for pointing that out.

* Various bug fixes contributed by Wilfred Hughes, 2*yo and Adam Goodman.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 58: https://github.com/drkjam/netaddr/issues/58

    - foo.bar doesn't throw AddrFormatError

FIXED Issue 57: https://github.com/drkjam/netaddr/issues/57

    - netaddr packages not hosted on PyPI

FIXED Issue 56: https://github.com/drkjam/netaddr/issues/56

    - Fix comparison with large IPSet()

FIXED Issue 55: https://github.com/drkjam/netaddr/pull/55

    - Fix smallest_matching_cidr and all_matching_cidrs

FIXED Issue 53: https://github.com/drkjam/netaddr/issues/53

    - Exclude 128.0.0.0/16 and possibly others from reserved range set?

FIXED Issue 51: https://github.com/drkjam/netaddr/issues/51

    - Encoding errors in netaddr/eui/oui.txt

FIXED Issue 46: https://github.com/drkjam/netaddr/issues/46

    - len(IPSet()) fails on python3

FIXED Issue 43: https://github.com/drkjam/netaddr/issues/43

    - Method to check if IPSet is contiguous

FIXED Issue 38: https://github.com/drkjam/netaddr/issues/38

    - netmask_bits is missing from the IPAddress

FIXED Issue 37: https://github.com/drkjam/netaddr/issues/37

    - Test failures with Python 3.3


---------------
Release: 0.7.10
---------------
Date: 6th Sep 2012

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.9
^^^^^^^^^^^^^^^^^^^

* A bunch of Python 3.x bug fixes. Thanks Arfrever.

* Extended nmap support to cover full target specification.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 36 - https://github.com/drkjam/netaddr/issues/36

- ResourceWarnings with Python >=3.2

FIXED Issue 35 - https://github.com/drkjam/netaddr/issues/35

- netaddr-0.7.9: Test failure with Python 3

FIXED Issue 34 - https://github.com/drkjam/netaddr/issues/34

- netaddr.ip.iana.SaxRecordParser.endElement() incompatible with Python 3.1

FIXED Issue 33 - https://github.com/drkjam/netaddr/issues/33

- netaddr script not installed with Python 3

FIXED Issue 23 - https://github.com/drkjam/netaddr/issues/23

- valid_nmap_range() does not validate nmap format case.

FIXED Issue 22 - https://github.com/drkjam/netaddr/issues/22

- all_matching_cidrs: documentation incorrect

--------------
Release: 0.7.9
--------------
Date: 28th Aug 2012

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.8
^^^^^^^^^^^^^^^^^^^

* Re-release to fix build removing Sphinx dependency.

--------------
Release: 0.7.8
--------------
Date: 28th Aug 2012

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.7
^^^^^^^^^^^^^^^^^^^

* New SAX parser for IANA data source files (contributed by Andrew Stromnov)

* Fixed pickling failures with EUI, OUI and IAB classes.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 31 - https://github.com/drkjam/netaddr/issues/31

    - Exclude '39.0.0.0/8' network from reserved set. Thanks Andrew Stromnov

FIXED Issue 28 - https://github.com/drkjam/netaddr/issues/28

    - Fix algorithm in ipv6_link_local to fully conform to rfc4291. Thanks Philipp Wollermann

FIXED Issue 25 - https://github.com/drkjam/netaddr/issues/25

    - install_requires is too aggressive? Thanks Adam Lindsay and commenters.

FIXED Issue 21 - https://github.com/drkjam/netaddr/issues/21

    - deepcopy for EUI fails. Thanks Ryan Nowakowski.

--------------
Release: 0.7.7
--------------
Date: 30th May 2012

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.6
^^^^^^^^^^^^^^^^^^^

* Comprehensive documentation update! It's only taken 4 years
  to get around to using Sphinx and I can confirm it is
  **TOTALLY AWESOME!**

* Various bug fixes

* Refreshed IEEE OUI and IAB data

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 24 - https://github.com/drkjam/netaddr/issues/24

    - Fixed TypeError when comparing BaseIP instance with non-BaseIP objects. Thanks pvaret

FIXED Issue 17 - https://github.com/drkjam/netaddr/issues/17

    - For large ipv6 networks the .subnet() method fails. Thanks daveyss

FIXED Issue 20 - https://github.com/drkjam/netaddr/issues/20

    - Test failure with Python 3. Thanks Arfrever

--------------
Release: 0.7.6
--------------
Date: 13th Sep 2011

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.5
^^^^^^^^^^^^^^^^^^^

* A bug fix point release

* Refreshed 3rd party data caches

* Tested against Python 3.2.x and PyPy 1.6.x

* Fixed unit tests under for Mac OSX

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 15 - https://github.com/drkjam/netaddr/issues/15

    - Incorrect and invalid glob produced when last octet is not *

FIXED Issue 13 - https://github.com/drkjam/netaddr/issues/13

    - Added support for IPython 0.11 API changes. Thanks juliantaylor

FIXED Issue 11 - https://github.com/drkjam/netaddr/issues/11

    - Calling valid_glob on cidr raises ValueError. Thanks radicand

FIXED Issue 7 - https://github.com/drkjam/netaddr/issues/7

    - Unpickling Bug in IPSet. Thanks LuizOz and labeneator

FIXED Issue 2 - https://github.com/drkjam/netaddr/issues/2

    - UnboundLocalError raised in IPNetwork constructor. Thanks keesbos

^^^^^^^^^^^
Miscellanea
^^^^^^^^^^^

- Has a famous soft drink company started making it own NICs?

--------------
Release: 0.7.5
--------------

Date: 5th Oct 2010

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.4
^^^^^^^^^^^^^^^^^^^

* Python 3.x is now fully supported. The paint is still drying on this so
  please help with testing and raise bug tickets when you find any issues!
  New Issue Tracker - https://github.com/drkjam/netaddr/issues

* Moved code hosting to github. History ported thanks to svn2git.
  - (https://github.com/nirvdrum/svn2git)

* All netaddr objects now use approx. 65% less memory due to the use of
  __slots__ in classes throughout the codebase. Thanks to Stefan Nordhausen
  and his Python guru for this suggestion!

* Applied many optimisations and speedups throughout the codebase.

* Fixed the behaviour of the IPNetwork constructor so it now behaves in
  a much more sensible and expected way (i.e. no longer uses inet_aton
  semantics which is just plain odd for network addresses).

* One minor change to behaviour in this version is that the .value property
  on IPAddress and IPNetwork objects no longer support assignment using a
  string IP address. Only integer value assignments are now valid. The impact
  of this change should be minimal for the majority of users.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 49 - http://code.google.com/p/netaddr/issues/detail?id=49

    - Incorrect IP range recognition on IPs with leading zeros

FIXED Issue 50 - http://code.google.com/p/netaddr/issues/detail?id=50

    - CIDR block parsing

FIXED Issue 52 - http://code.google.com/p/netaddr/issues/detail?id=52

    - ipv6 cidr matches incorrectly match ipv4 [sic]

FIXED Issue 53 - http://code.google.com/p/netaddr/issues/detail?id=53

    - Error in online documentation

FIXED Issue 54 - http://code.google.com/p/netaddr/issues/detail?id=54

    - IP recognition failure 

FIXED Issue 55 - http://code.google.com/p/netaddr/issues/detail?id=55

    - Support for Python 3.x

FIXED Issue 56 - http://code.google.com/p/netaddr/issues/detail?id=56

    - checking IPAddress in IPNetwork

FIXED Issue 57 - http://code.google.com/p/netaddr/issues/detail?id=57

    - netaddr objects can't pickle

FIXED Issue 58 - http://code.google.com/p/netaddr/issues/detail?id=58

    - IPSet operations should accept the same arguments as IPAddress

FIXED Issue 59 - http://code.google.com/p/netaddr/issues/detail?id=59

    - netaddr fails to load when imported by a PowerDNS coprocess

^^^^^^^^^^^
Miscellanea
^^^^^^^^^^^

- Welcome back to standards.ieee.org which seems to have been down for weeks!

- Goodbye Sun Microsystems + Merrill Lynch, hello Oracle + Bank of America ...

--------------
Release: 0.7.4
--------------
Date: 2nd Dec 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.3
^^^^^^^^^^^^^^^^^^^

* Applied speed patches by S. Nordhausen

* Fixed an inconsistency between EUI and IPAddress interfaces. Made
  EUI.packed and EUI.bin properties (previously methods) and added a
  words() property.

--------------
Release: 0.7.3
--------------
Date: 14th Sep 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.2
^^^^^^^^^^^^^^^^^^^

* Added __add__, __radd__, __sub__, __rsub__ operators to the IPAddress class.

* Added support for validation and iteration of simple nmap style IPv4 ranges
  (raised in Issue 46).

* Removed some unused constants from fallback socket module.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 44 - http://code.google.com/p/netaddr/issues/detail?id=44

    - int/long type error

FIXED Issue 46 - http://code.google.com/p/netaddr/issues/detail?id=46

    - Question about IPv4 ranges

FIXED Issue 47 - http://code.google.com/p/netaddr/issues/detail?id=47

    - IPNetwork cannot be evaluated as a boolean when it has a large size

--------------
Release: 0.7.2
--------------
Date: 20th Aug 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.7.1
^^^^^^^^^^^^^^^^^^^

FIXED a boundary problem with the iter_iprange() generator function
  and all associated calls to it throughout the codebase, including
  unit test coverage and adjustments.

* Replaced regular expressions in cidr_merge() with pre-compiled equivalents
  for a small speed boost.

* Adjustments to README raised by John Eckersberg.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 43 - http://code.google.com/p/netaddr/issues/detail?id=43

    - IPNetwork('0.0.0.0/0') not usable in for loop

--------------
Release: 0.7.1
--------------
Date: 14th Aug 2009

^^^^^^^^^^^^^^^^^
Changes since 0.7
^^^^^^^^^^^^^^^^^

* Renamed the netaddr shell script from 'nash' to plain 'netaddr'. This
  is to avoid a potentially nasty clash with an important Linux tool
  with the same name.
  
  Thanks to John Eckersberg for spotting this one early!

* Updated IANA and IEEE data files with latest versions.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 42 - http://code.google.com/p/netaddr/issues/detail?id=42

    - Bug in cidr_merge() function when passed the CIDRs 0.0.0.0/0 and/or ::/0

------------
Release: 0.7
------------
Date: 11th Aug 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.6.x
^^^^^^^^^^^^^^^^^^^

Please Note - This release represents a major overhaul of netaddr. It breaks
backward compatibility with previous releases. See the API documentation for
full details of what is available.

Some highlights of what has changed :-

* Internal module hierarchy has been completely overhauled and redesigned. This
  fixes up a lot of inconsistencies and problems with interdependent imports.
  All public classes, objects, functions and constants are still published via
  the main netaddr module namespace as in previous releases.

* No more AT_* and ST_* 'constants'.

* The Addr base class is gone. This removes the link between EUI and IP
  functionality so the library is can now easily be split into distinct units
  without many interdependencies between layer 2 and layer 3 functionality.

* The use of custom descriptor classes has been completely discontinued.

* Strategy classes and singleton objects have been replaced with a group of
  strategy modules in their own netaddr.strategy namespace. Each IP or EUI
  address object now holds a reference to a module rather than a singleton
  object.

* Many operations that were previously static class methods are now presented as
  functions in the relevant modules. See the API documentation for details.
  
* The IP and CIDR classes have been replaced with two new classes called
  IPAddress and IPNetwork respectively. This name change is important as the IP
  part of netaddr has been completed redesigned. The notion of an individual IP
  address and an IP network or subnet has been made more obvious. IPAddress
  objects are now true scalars and do not evaluate in a list or tuple context.
  They also do not support any notion of a netmask or CIDR prefix; this is the
  primary function of an IPNetwork object.

* Arbitrary IP ranges and are still supported but a lot of their functionality
  has also been exposed via handy functions.

* IP globbing routines (previous known as Wildcards) have been moved into
  their own submodule. 

* Added a new IPSet class which fully emulates mutable Python sets. This
  replaces a lot of half-baked experimental classes found in 0.5.x and 0.6.x
  such as IPRangeSet and CIDRGroup. See documentation for details.

* All methods and properties that previously used or supported the 'fmt'
  formatting property no longer do so. In all cases, objects are now returned to
  correctly support pass through calls without side effects. It is up to the
  user to extract data in the right format from the objects IPAddress objects
  returned as required.

* Unit tests have been completed re-written to support docstring style tests
  bundled into test suites. These are handy as they double up as documentation
  being combined with wiki syntax. Implemented code coverage checking using
  coverage 3.x.

* nash - a nascent shell like tool for the netaddr library (requires IPython).

* Support for RFC 1924 added ;-)

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 13 - http://code.google.com/p/netaddr/issues/detail?id=13

    - Searching for a match in a list of CIDR objects

FIXED Issue 26 - http://code.google.com/p/netaddr/issues/detail?id=26

    - Refactor out use of isinstance()

FIXED Issue 28 - http://code.google.com/p/netaddr/issues/detail?id=28

    - Add support for network block operations

FIXED Issue 34 - http://code.google.com/p/netaddr/issues/detail?id=34

    - Addition issue?

--------------
Release: 0.6.4
--------------
Date: 11th Aug 2009

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 40 - http://code.google.com/p/netaddr/issues/detail?id=40

    - Building RPM with "python setup.py bdist_rpm" fails, multiple errors

--------------
Release: 0.6.3
--------------
Date: 23rd Jun 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.6.2
^^^^^^^^^^^^^^^^^^^

* Fixed line endings in a number of new files created under Windows.

* Tweaked the ordering of values in tuple passed into the hash() function in 
  the __hash__ method of the IP and IPRange classes to make it the same as 
  the values used for comparisons implemented in the __eq__ method (Python
  best practice).

* Added a number of unit tests to improve code coverage.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 33 - http://code.google.com/p/netaddr/issues/detail?id=33

    - CIDR subtraction is broken for out-of-range CIDR objects

FIXED Issue 35 - http://code.google.com/p/netaddr/issues/detail?id=35

    - install error (on Python interpreters where socket.has_ipv6 is False)

FIXED Issue 36 - http://code.google.com/p/netaddr/issues/detail?id=36

    - netaddr.CIDR fails to parse default route CIDR

FIXED Issue 37 - http://code.google.com/p/netaddr/issues/detail?id=37

    - Bug in bitwise AND operator for IP addresses

FIXED Issue 38 - http://code.google.com/p/netaddr/issues/detail?id=38

    - Feature request: Addr.__nonzero__

FIXED Issue 39 - http://code.google.com/p/netaddr/issues/detail?id=39

    - CIDR.abbrev_to_verbose() not applying implicit classful netmask
      rules consistently

--------------
Release: 0.6.2
--------------
Date: 13th Apr 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.6.1
^^^^^^^^^^^^^^^^^^^

* Refreshed IEEE and IANA data files with latest revisions from their 
  respective URLs.

  - IANA IPv4 Address Space Registry (last updated 2009-03-11)

  - Internet Multicast Addresses (last updated 2009-03-17)

  - IEEE OUI and IAB files (last updated 2009-04-13)

* Added get_latest_files() functions to both the netaddr.eui and 
  netaddr.ip modules to assist in automating release builds.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 32 - http://code.google.com/p/netaddr/issues/detail?id=32

    - Addr.__ne__ returns wrong answer

--------------
Release: 0.6.1
--------------
Date: 6th Apr 2009

^^^^^^^^^^^^^^^^^
Changes since 0.6
^^^^^^^^^^^^^^^^^

* Added COPYRIGHT file with details and attribution for all 3rd party files
  bundled with netaddr.

* Minimum Python version required is now 2.4.x changed from 2.3.x.
  
  - Python 2.3 compatibility code in many sections of code have been removed.
  
  - the @property and @staticmethod decorators are now used throughout the 
    code along with the reversed() and sorted() builtin iterators.
    
  - A specific version check has also been added that will raise RuntimeError
    exceptions if you run netaddr on a Python interpreter version < 2.4.x.

* Integer addresses passed to the IP() and EUI() constructors no longer 
  require a mandatory second address type (AT_*) argument in most cases. This
  is now only really required to disambiguate between IPv4/IPv6 addresses with
  the same numerical value. The same behaviour applies to EUI-48/EUI-64 
  identifiers. A small speed boost is achieved if the 2nd address type 
  argument is explicitly provided.
  
* IPv6 addresses returned by EUI.ipv6_link_local() now always have a subnet
  prefix of /64.

* Default sort order of aggregate classes (IPRange, CIDR and Wildcard) has
  been changed (again). They now sort initially by first address and then
  by network block size from largest to smallest which feels more natural.

* Fixed a bug in the CIDR.abbrev_to_verbose() static method where IPv4 
  addresses with 4 octets (i.e. non-partial addresses) were being assigned
  subnet prefixes using abbreviated rules. All complete IPv4 addresses should
  always get a /32 prefix where it is not explicitly provided.

* Abbreviated address expansion in the CIDR constructor is now optional and
  can be controlled by a new 'expand_abbrev' boolean argument.

* Added the new CIDR.summarize() static method which transforms lists of IP
  addresses and CIDRs into their most compact forms. Great for trimming down
  large ad hoc address lists!

* Added the previous() and next() methods to the CIDR classes which return
  the CIDR subnets either side of a given CIDR that are of the same size.
  For the CIDR 192.0.2.0/24, previous will return 192.0.1.0/24 and next
  will return 192.0.3.0/24. Also accepts and optional step size (default
  is 1).
  
* Added the supernet() method to the CIDR class which returns a generator of
  all the subnets that contain the current CIDR found by decrementing the
  prefixlen value for each step until it reaches zero.

* Changed the way the fallback code works when the socket module is missing
  important constants and functions.

* Removed the uppercase options from the Strategy constructors and internals
  as this behaviour can be easily replicated using the word_fmt option 
  instead and requires less code (word_fmt='%X').

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FIXED Issue 23 - http://code.google.com/p/netaddr/issues/detail?id=23

    - Improve IPv6 IPv4 mapped/compatible address formatting

FIXED Issue 24 - http://code.google.com/p/netaddr/issues/detail?id=24

    - bug in CIDR.subnet() when using the fmt argument

FIXED Issue 29 - http://code.google.com/p/netaddr/issues/detail?id=29

    - CIDR.subnet method's count argument isn't working as documented

FIXED Issue 30 - http://code.google.com/p/netaddr/issues/detail?id=30

    - not compatible with Python 2.3

FIXED Issue 31 - http://code.google.com/p/netaddr/issues/detail?id=31

    - byte order in documentation confusing or wrong

------------
Release: 0.6
------------
Date: 20th Jan 2009

^^^^^^^^^^^^^^^^^^^
Changes since 0.5.x
^^^^^^^^^^^^^^^^^^^

* Namespace changes

  3 new sub namespaces have been added :-

  - netaddr.eui
  
  Currently contains IEEE OUI and IAB classes and lookup code.
  
  - netaddr.ip

  Currently contains IANA IPv4, IPv6 and IPv4 multicast lookup code.

  - netaddr.core

  Currently contains only a couple of classes that are shared between code in 
  netaddr.eui and netaddr.ip.

  Please Note: This change is part of a two stage internal restructuring of
  netaddr. In future releases, layer-2 MAC/EUI functionality will be separated
  from and layer-3 IP, CIDR and Wildcard functionality. All shared code will 
  be moved to netaddr.core. When the migration is complete (expected in 0.7)
  the netaddr.address and netaddr.strategy namespaces will be removed. Please
  endeavour to access everything you need via the top-level netaddr namespace
  from this release onwards. See netaddr.__all__ for details of constants, 
  objects, classes and functions intended for the public interface.

* Addition of IEEE and IANA informational lookups

  - the IP() and EUI() classes now have an additional info() method through 
    which contextual information about your addresses can be accessed. This
    data is published by IANA and the IEEE respectively and sourced directly 
    from text files bundled with netaddr that are available for download
    publicly online. Details are available in the docstring of the relevant
    parsing classes. Subsequent netaddr releases will endeavour to keep 
    up-to-date with any updates to these files.
    
  - the EUI() class has been updated with the addition of the OUI() and IAB()
    classes. They provide object based access to returned via the EUI.info()
    method. Please see API docs included with netaddr for details.
    
  - added new NotRegisteredError exception that is raised when an EUI doesn't
    match any currently registration entries in the IEEE registry files.

* Addr() class removed from the public interface

  - This class is only ever meant to be used internally and its usage may soon
    be deprecated in favour converting it into an abstract base class in 
    future releases.
    
* Deletion of AddrRange() class

  - replaced with the more specific IPRange() class. AddrRange() wasn't
    very useful in practice. Too much time has been spent explaining its 
    theoretical merits over its actual practicality for every day use.

* Addition of new IPRange() class

  - the new base class for CIDR() and Wildcard().

  - a 'killer feature' of this new class are the new methods iprange(),
    cidrs() and wildcard() which allow you to use and switch between all 
    3 formats easily. IPRange('x', 'y').cidrs() is particularly useful
    returning all the intervening CIDRs between 2 arbitrary IP addresses.

  - IPRange() is a great place to expose several new methods available to
    sub classes. They are issupernet(), issubnet(), adjacent() and overlaps().

  - previous method called data_flavour() has been renamed (again) to a more
    suitable format().

* IP() class updates
  
  - is_netmask() and is_hostmask() methods have been optimised and are now 
    both approximately 4 times faster than previously!

  - added wildcard() and iprange() methods that return pre-initialised
    objects of those classes based on the current netmask / subnet prefix.

  - copy constructor methods ipv4() and ipv6() now preserve the value of the
    prefixlen property now also support IPv6 options for returning IPv4-mapped
    or IPv4-compatible IPv6 addresses.

  - added new methods is_loopback(), is_private(), is_link_local(), 
    is_ipv4_mapped() and is_ipv4_compat() which are all self explanatory.
    
  - added a bin() method which provides an IP address in the same format 
    as the standard Python bin() builtin type ('0bxxx') now available in 
    Python 2.6.x and higher.
    
  - added a packed() method which provides an IP address in packed binary 
    string format, suitable for passing directly to Python socket calls.
    
* nrange() generator function updates

  - by default this now returns IP() objects instead of Addr() objects.

* CIDR() class updates

  - the 'strict_bitmask' option in the CIDR class constructor has been had a
    name change and is now just 'strict' (less typing).

  - support for Cisco ACL-style (hostmask) prefixes. Also available to the 
    IP() class. They are converted to their netmask equivalents before being 
    applied to the base address.

  - added a new subnet() generator method that returns iterators to subnet 
    CIDRs found within the current CIDR object's boundaries e.g. a /24 CIDR
    can provide address with subnet prefixes between a /25 and /32.

  - added a new span() method which takes a list of IP, IPRange, CIDR and/or 
    Wildcards returning a single CIDR that 'spans' the lowest and highest 
    boundary addresses. An important property of this class is that only a 
    single CIDR is returned and that it (potentially) overlaps the start and
    end addresses. The most important aspect of this method is that it 
    identifies the left-most set of bits that are common to all supplied 
    addresses. It is the plumbing that makes a lot of other features function
    correctly.

  - although IPv6 doesn't support the concept of a broadcast address, after 
    some pondering I've decide to add network() and broadcast() methods to the
    CIDR class. It is an interface quirk that users expect so it has been 
    added for ease of use.
    
  - the methods network(), broadcast(), hostmask() and netmask() have been 
    wrapped in property() builtin calls to make them appear as read-only
    properties.

* Many more MAC and IPv4 string address representation are now supported

  - Improvements to both EUI and IP classes. They now accept many more valid
    address formats than previously. Thanks for all the bugs tickets raised.

* ``__repr__()`` method behaviour change

  - Using ``repr()`` now assume that you have performed a ``from netaddr import *``
    before you execute them. They no longer specify the originating namespace
    of objects which is a bit unnecessary and a lot to read on-screen.They 
    will also be moving around within the namespace shortly anyway so its
    best not to think of them as being anywhere other than directly below
    netaddr itself.

* 'klass' property renamed to 'fmt' (format)

  - now referred to as the 'format callable' property. An unfortunately but
    necessary change. 'klass' was a bad initial name choice as it most often
    doesn't even reference a class object also supporting references to Python
    types, builtin functions and user defined callables.

* Complete re-work and consolidation of unit tests.

  - now over 100 tests covering all aspects of the API and library
    functionality.
  - Moved all tests into a single file. Lots of additional tests have been
    added along with interface checks to ensure netaddr's always presents
    a predictable set of properties and methods across releases.

* Nascent support for Python eggs and setuptools.

  - Help is need to test this as it is not something I use personally.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Finally fixed the IPv6 string address compression algorithm so that it 
  is now compliant with the socket modules inet_ntop() and inet_pton() calls.
  (not available on all platforms).

^^^^^^^^^^^^^^^^^^^^^
Experimental Features
^^^^^^^^^^^^^^^^^^^^^

* added bitwise operators to the IP class

  - does what it says on the tin. Does not effect that value of the IP object
    itself but rather, returns a new IP after the operation has been applied.

* IPRangeSet() class added (EXPERIMENTAL).

  - the intention with this class is to allows you to create collections of
    unique IP(), IPRange(), CIDR() and Wildcard() objects. It provides 
    iteration over IPs in the collection as well as several membership based
    operations such as any_match() all_matches(), min_match() and max_match().
  - lots more work to do here. Please raise bugs and feature requests against
    this as you find them. Improvements to this are coming in 0.7.

--------------
Release: 0.5.2
--------------
Date: 29th Sep 2008

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed Issue 15 in bug tracker. Bad validation and conversion of IPv4 
  mapped IPv6 address values in IPv6Strategy class. Covered with unit
  test cases.
   
* Updated PrefixLenDescriptor() class so that modifications to the property
  CIDR.prefixlen also update CIDR.first and CIDR.last keeping them in sync.
  Covered by unit test cases.

* IP.hostname() method returns None when DNS lookup fails.

--------------
Release: 0.5.1
--------------
Date: 23rd Sep 2008

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specific bug fixes addressed in this release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* CIDR constructor was throwing a TypeError for valid unicode string addresses
  which worked in previous releases. Fixed and covered with a unit test case.
  
* The methods CIDR.netmask() and CIDR.hostmask() contained code errors that
  were causing them to fail. Problem fixed and covered with unit test case.

------------
Release: 0.5
------------
Date: 19th Sep 2008

^^^^^^^^^^^^^^^^^^^
Changes since 0.4.x
^^^^^^^^^^^^^^^^^^^

**General**

* Access to all important object attributes in all netaddr classes now takes
  place via custom Python descriptor protocol classes. This has greatly 
  simplified internal class logic and made external attributes changes much
  safer and less error prone. It has also made aggregate classes such as CIDR
  and Wildcard effectively read-write rather than read-only which they have 
  been up until this release.

* Amended the way sort order is calculated for Addr and AddrRange (sub)class
  instances so that the address type is taken into account as well as as the 
  numerical value of the address or address range. The ascending sort order 
  is IPv4, IPv6, EUI-48 and EUI-64. Sequences of AddrRange (sub)class 
  instances now sort correctly! 
  
* Comparisons between instances of Addr and AddrRange (sub)classes now return 
  False, rather than raising an AttributeError. 
  
* Added checks and workaround code for Python runtime environments that suffer
  from the infamous socket module inet_aton('255.255.255.255') bug. This was
  discovered recently in Python 2.4.x on PowerPC under MacOS X. The fix also
  applies in cases where the socket module is not available (e.g. on Google
  App Engine).

* All general Exception raising in the strategy module has now been replaced 
  with more specific exceptions, mainly ValueError (these were unintentionally
  missed out of the 0.4 release).

* Implemented __hash__() operations for the Addr and AddrStrategy classes. This 
  allows you to use IP, CIDR and Wildcard objects as keys in dictionaries and
  as elements in sets. Please note - this is currently an experimental feature
  which may change in future releases.

* Added __ne__() operation to Addr and AddrRange classes.

* Obeying the 'Law of Demeter', the address type of Addr and AddrRange 
  (sub)class instances can be accessed using the property directly :-
  
    obj.addr_type  # 0.5 onwards
    
  rather than having to go via the strategy object :- 
  
    obj.strategy.addr_type  # 0.4 and earlier

* Renamed the AT_DESCR lookup dictionary to AT_NAMES. Removed invalid and 
  duplicated imports from all modules.

**Addr class changes**

* Removed the setvalue() method from the Addr class and replaced all uses of 
  __setattr__() replaced by custom descriptors throughout.

**IP class changes**

* Removed the ambiguity with masklen and prefixlen attributes in the IP class.
  prefixlen now denotes the number of bits that define the netmask for an IP
  address. The new method netmask_bits() returns the number of non-zero bits 
  in an IP object if the is_netmask() method returns True. A prefixlen value
  other than /32 for an address where is_netmask() returns True is invalid 
  and will raise a ValueError exception.

* Removed the family() method from the IP class. It duplicates information
  now provided by the prefixlen property.

* IP class has several new methods. is_multicast() and is_unicast() quickly 
  tell you what category of IP address you have and while ipv4() and ipv6() 
  act as IPv4 <-> IPv6 conversions or copy constructors depending on context.
  
* Reverse DNS lookup entries now contain a trailing, top-level period (.)
  character appended to them.
  
* Added the hostname() method to IP instances which performs a reverse DNS

* The IP class __str__() method now omits the subnet prefix is now implicit 
  for IPv4 addresses that are /32 and IPv6 addresses that are /128. Subnet
  prefix is maintained in return value for all other values.

**AddrRange class changes**

* The AddrRange class no longer stores instances of Addr (sub)classes for the 
  first and last address in the range. The instance variables self.start_addr
  and self.stop_addr have been renamed to self.first and self.last and the 
  methods obj.first() and obj.last() have been removed.
  
  Instead, self.first and self.last contain integer values and a reference 
  to a strategy object is stored. Doing this is a lot more useful and cleaner 
  for implementing internal logic.
  
  To get Addr (sub)class objects (or strings, hex etc when manipulating the
  the klass property) use the index values obj[0] and obj[-1] as a substitute
  for obj.first() and obj.last() respectively.
    
* AddrRange (sub)class instances now define the increment, __iadd__(), and 
  decrement, __isub__(), operators. This allows you to 'slide' CIDRs and 
  Wildcards upwards and downwards based on their block sizes.

* The _retval() method has now been renamed data_flavour() - yes, the UK 
  spelling ;-) You shouldn't really care much about this as it mostly for 
  internal use. I gave it a decent name as I didn't see any real need to hide
  the functionality if users wanted it.

**CIDR class changes**

* The strictness of the CIDR class constructor in relation to non-zero bits
  once the prefix bitmask has been applied can be disabled use the optional
  argument strict_bitmask=False. It is True (strictness enabled) by default.

* Fixed a bug in abbreviated CIDR conversion. Subnet prefix for multicast 
  address 224.0.0.0 is now /4 instead of /8.

* The CIDR class now supports subtraction between two CIDR objects, returning
  a list of the remainder. Please note that the bigger of the two CIDR objects
  must be on the left hand side of the the expression, otherwise an empty list
  is return. Sorry, you are not allowed to create negative CIDRs ;-)

* The function abbrev_to_cidr() has been renamed to and turned into the static 
  method CIDR.abbrev_to_verbose(). No major changes to the logic have been
  made.
  
**Wildcard class changes**

* The Wildcard class now defines a static method Wildcard.is_valid() that 
  allows you to perform validity tests on wildcard strings without fully 
  instantiation a Wildcard object.

------------
Release: 0.4
------------
Date: 7th Aug 2008

^^^^^^^^^^^^^^^^^^^
Changes since 0.3.x
^^^^^^^^^^^^^^^^^^^

* All general Exception raising has been replaced with more specific 
  exceptions such as TypeError and ValueError and with the addition of two 
  custom exception classes, AddrFormatError and AddrConversionError.

* The IP class now accepts a subnet prefix. It is *NOT* strict about non-zero
  bits to the right of implied subnet mask, unlike the CIDR class (see below).

* The CIDR class is now completely strict about non-zero bits to the right of 
  the implied subnet netmask and raises a ValueError if they exist, with a 
  handy hint as to the correct CIDR to be used based on the supplied subnet 
  prefix.

* The CIDR class now also supports abbreviated CIDR ranges and uses older
  classful network address rules to decided on a subnet prefix if one is not
  explicitly provided. Supported forms now include 10, 10/8 and 192.168/16.
  Currently only supports these options for IPv4 CIDR address ranges.

* __repr__() methods have been defined for all classes in the netaddr module 
  producing executable Python statements that can be used to re-create the
  state of any object.

* CIDR and Wildcard classes now have methods that support conversions between 
  these two aggregate types :-
    
    * CIDR -> Wildcard
    * Wildcard -> CIDR
    
^^^^^^^^^^^^^^^^^^^^
Housekeeping Changes
^^^^^^^^^^^^^^^^^^^^

* Massive docstring review and tidy up with the inclusino of epydoc specific 
  syntax to spruce up auto-generated API documentation.

* Thorough review of code using pylint.

* Netaddr module now has the special __version__ variable defined which is 
  also referenced by setup.py.

* Some minor changes to setup.py and MANIFEST.in.

* Constants and custom Exception classes have been moved to __init__.py from
  strategy.py
  
* An import * friendly __all__ has been defined for the netaddr namespace
  which should remove the need to delve too much into the address and strategy
  submodules.
  
* Fixed a number of line-ending issues in several files.

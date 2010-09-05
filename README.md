# About

I found it a pain to maintain multiple views in bind because I had to constantly
modify two files (internal and external views).

Once I started using IPv6 I had another grief, I had to update IPv4 and IPv6 and CNAME
records in two files.

This script reads from a metazone file and generates internal/external zone files.  You
can then use bind's *acl* and *view* features to craft what internal and external
hosts see.

# Running

There are a few dependencies

 - perl with Parse::RecDescent

You may want to run this on Debian:

    apt-get install libparse-recdescent-perl

There is a simple.metazone file that comes with the tool.  Run it like so:

        zonegen --selector int simple.metazone
        cat example.com.int.zone

        zonegen --selector ext simple.metazone
        cat example.com.ext.zone

# Example

For example, given

        foo: int=10.0.0.1 ext=72.0.0.1 2001:1234::1
        www: foo

You'll get this in the internal zone:

        foo.ipv6             IN AAAA    2001:1234::1
        foo.ipv4             IN A       10.0.0.1
        foo                  CNAME      foo.ipv6
        foo                  CNAME      foo.ipv4
        www                  CNAME      baz

and the external zone will contain:

        foo.ipv6             IN AAAA    2001:1234::1
        foo.ipv4             IN A       72.0.0.1
        foo                  CNAME      foo.ipv6
        foo                  CNAME      foo.ipv4
        www                  CNAME      foo

<!-- vim: set ft=mkd -->

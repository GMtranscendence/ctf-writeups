# Port Labyrinth

```
Прапор був розкиданий між багатьма TCP-портами. Чи знайдете ви всі частини?

Зауваження: Не шукайте відомі порти, почніть пошук з більш високого номера порту.

```

There are 65535 available ports. The task gives us a hint that we should not look for the common ports but rather, for the higher ones. So we make a guess about its range and scan the server with __nmap__ to look for open ports

```
[GMtranscendence] $ nmap -p 60000-65535 10.123.45.102

Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-28 17:15 EEST
Nmap scan report for 10.123.45.102
Host is up, received conn-refused (0.066s latency).
Not shown: 5504 closed tcp ports (conn-refused)
PORT      STATE SERVICE REASON
61000/tcp open  unknown syn-ack
61001/tcp open  unknown syn-ack
61002/tcp open  unknown syn-ack
61003/tcp open  unknown syn-ack
61004/tcp open  unknown syn-ack
61005/tcp open  unknown syn-ack
61006/tcp open  unknown syn-ack
61007/tcp open  unknown syn-ack
61008/tcp open  unknown syn-ack
61009/tcp open  unknown syn-ack
61010/tcp open  unknown syn-ack
61011/tcp open  unknown syn-ack
61012/tcp open  unknown syn-ack
61013/tcp open  unknown syn-ack
61014/tcp open  unknown syn-ack
61015/tcp open  unknown syn-ack
61016/tcp open  unknown syn-ack
61017/tcp open  unknown syn-ack
61018/tcp open  unknown syn-ack
61019/tcp open  unknown syn-ack
61020/tcp open  unknown syn-ack
61021/tcp open  unknown syn-ack
61022/tcp open  unknown syn-ack
61023/tcp open  unknown syn-ack
61024/tcp open  unknown syn-ack
61025/tcp open  unknown syn-ack
61026/tcp open  unknown syn-ack
61027/tcp open  unknown syn-ack
61028/tcp open  unknown syn-ack
61029/tcp open  unknown syn-ack
61030/tcp open  unknown syn-ack
61031/tcp open  unknown syn-ack

Nmap done: 1 IP address (1 host up) scanned in 5.50 seconds
```

---

The first thought is to automate the process of connecting to those ports via a scripting language such as python or bash, but i decided to try to connect to the very last port to take a look what's there.

```
[GMtranscendence] $ nc 10.123.45.102 61031
Looks like you have found a map!
         _______________
    ()==(      UD{      (@==()
         '______________'|
           |   61018,    |
           |   61011,    |
           |   61019,    |
           |   61016,    |
           |   61013,    |
           |   61015     |
         __)_____________|
    ()==(        }      (@==()
         '--------------'%
```

Luckily for me its another hint of ports that we actually need to conect to.

---

We could connect to each port by hand, but it's actually faster to write a simple bash script to do it for us, and it scales pretty good for the future needs.

```
#!/usr/bin/env bash

ports=(61018 61011 61019 61016 61013 61015)
ip="10.123.45.102"

for port in "${ports[@]}"; do
  nc $ip $port
done
```

We start the script

```
[GMtranscendence] $ chmod 755 labyrinth.sh
[GMtranscendence] $ ./labyrinth.sh

p0rT_l4bYr1n7h
```

And we got our flag

---

## ctf{p0rT_l4bYr1n7h}


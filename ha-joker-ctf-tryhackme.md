---
icon: caret-right
description: 'HA Joker CTF: une solution en français'
---

# HA Joker CTF TryHackMe

<figure><img src=".gitbook/assets/joker-thm-thunder-01.png" alt=""><figcaption><p>HA Joker Thumbnail</p></figcaption></figure>

" _Batman hits Joker_"

[lien vers TryHackMe.com](https://tryhackme.com/r/room/jokerctf)

## \[Task 1] Enumerate Services

### [Nmap](outils.md#nmap)

```bash
# Nmap 7.94SVN scan initiated Fri Nov  8 10:54:00 2024 as: /usr/lib/nmap/nmap --privileged -A -p- -oN nmap.txt -T4 10.10.135.3
Nmap scan report for 10.10.135.3
Host is up (0.023s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ad:20:1f:f4:33:1b:00:70:b3:85:cb:87:00:c4:f4:f7 (RSA)
|   256 1b:f9:a8:ec:fd:35:ec:fb:04:d5:ee:2a:a1:7a:4f:78 (ECDSA)
|_  256 dc:d7:dd:6e:f6:71:1f:8c:2c:2c:a1:34:6d:29:99:20 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HA: Joker
8080/tcp open  http    Apache httpd 2.4.29
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 401 Unauthorized
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=11/8%OT=22%CT=1%CU=43629%PV=Y%DS=2%DC=T%G=Y%TM=672D
OS:DFDC%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=A)
OS:SEQ(SP=107%GCD=1%ISR=108%TI=Z%CI=I%TS=A)SEQ(SP=107%GCD=1%ISR=109%TI=Z%CI
OS:=I%II=I%TS=9)SEQ(SP=108%GCD=1%ISR=10A%TI=Z%CI=I%II=I%TS=A)OPS(O1=M508ST1
OS:1NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M50
OS:8ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T
OS:=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T
OS:2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=4
OS:0%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%
OS:Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=16
OS:4%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   21.80 ms 10.9.0.1
2   21.87 ms 10.10.135.3

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Nov  8 10:54:36 2024 -- 1 IP address (1 host up) scanned in 35.65 seconds

```

## \[Task 2] Bruteforce

john


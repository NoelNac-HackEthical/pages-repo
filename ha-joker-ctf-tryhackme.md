---
icon: caret-right
description: Challenge niveau medium du site TryHackMe
---

# \[HA Joker]\[CTF]\[TryHackMe]

<figure><img src=".gitbook/assets/joker-thm-thunder-01.png" alt=""><figcaption><p>HA Joker Thumbnail</p></figcaption></figure>

" _Batman hits Joker_"

[lien vers TryHackMe.com](https://tryhackme.com/r/room/jokerctf)

## \[Task 1] Enumerate Services

Commençons comme d'habitude avec les classiques Nmap et Gobuster

### [Nmap](outils.md#nmap)

```bash
┌──(kali㉿kali)-[~/THM/joker]
└─$ nmap $IP -A -p- -oN nmap.txt -T4    
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-11 16:03 CET
Nmap scan report for 10.10.56.137
Host is up (0.026s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ad:20:1f:f4:33:1b:00:70:b3:85:cb:87:00:c4:f4:f7 (RSA)
|   256 1b:f9:a8:ec:fd:35:ec:fb:04:d5:ee:2a:a1:7a:4f:78 (ECDSA)
|_  256 dc:d7:dd:6e:f6:71:1f:8c:2c:2c:a1:34:6d:29:99:20 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: HA: Joker
|_http-server-header: Apache/2.4.29 (Ubuntu)
8080/tcp open  http    Apache httpd 2.4.29
|_http-title: 401 Unauthorized
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
|_http-server-header: Apache/2.4.29 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=11/11%OT=22%CT=1%CU=32135%PV=Y%DS=2%DC=T%G=Y%TM=673
OS:21D05%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=A
OS:)SEQ(SP=103%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=A)SEQ(SP=103%GCD=1%ISR=10A%T
OS:I=Z%CI=I%II=I%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=
OS:M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68
OS:DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%
OS:DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A
OS:=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%D
OS:F=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O
OS:=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=
OS:G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   25.49 ms 10.9.0.1
2   25.52 ms 10.10.56.137

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.97 seconds

```

### [Gobuster](outils.md#gobuster)

```
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt" -u $IP -t50 
```

```sh
┌──(kali㉿kali)-[~/THM]
└─$ gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt" -u $IP -t50     
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.56.137
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              txt,php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 310] [--> http://10.10.56.137/img/]
/.html                (Status: 403) [Size: 277]
/.php                 (Status: 403) [Size: 277]
/index.html           (Status: 200) [Size: 5954]
/css                  (Status: 301) [Size: 310] [--> http://10.10.56.137/css/]
/secret.txt           (Status: 200) [Size: 320]
/.html                (Status: 403) [Size: 277]
/.php                 (Status: 403) [Size: 277]
/phpinfo.php          (Status: 200) [Size: 94758]
/server-status        (Status: 403) [Size: 277]
Progress: 663797 / 882244 (75.24%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 663902 / 882244 (75.25%)
===============================================================
Finished
===============================================================
```

### Réponses au questions

<table><thead><tr><th width="583">Question</th><th>Réponse</th></tr></thead><tbody><tr><td>What version of Apache is it?</td><td>2.4.29</td></tr><tr><td>What port on this machine not need to be authenticated by user and password?</td><td>80</td></tr><tr><td>There is a file on this port that seems to be secret, what is it?</td><td>secret.txt</td></tr><tr><td>There is another file which reveals information of the backend, what is it?</td><td>phpinfo.php</td></tr><tr><td>When reading the secret file, We find with a conversation that seems contains at least two users and some keywords that can be intersting, what user do you think it is?</td><td>joker</td></tr><tr><td>What port on this machine need to be authenticated by Basic Authentication Mechanism?</td><td>8080</td></tr></tbody></table>



## \[Task 2] Bruteforce

john

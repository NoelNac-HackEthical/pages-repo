---
icon: caret-right
description: Challenge niveau medium du site TryHackMe
---

# \[Anonymous]\[CTF]\[TryHackMe]

<figure><img src=".gitbook/assets/THM-thumbnail-anonymous-under-construction-stamp.png" alt=""><figcaption><p>Anonymous Thumbnail</p></figcaption></figure>

**En cours de rédaction**

Lien vers TryHackMe: [https://tryhackme.com/r/room/anonymous](https://tryhackme.com/r/room/anonymous)

## \[Task 1] Enumérations

Commençons par les énumérations classiques

### 1. Nmap

```sh
# Nmap 7.94SVN scan initiated Thu Dec  5 10:43:13 2024 as: /usr/lib/nmap/nmap --privileged -A -p- -oN nmap.txt -T4 10.10.42.241
Nmap scan report for 10.10.42.241
Host is up (0.028s latency).
Not shown: 65531 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.2.92
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts [NSE: writeable]
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8b:ca:21:62:1c:2b:23:fa:6b:c6:1f:a8:13:fe:1c:68 (RSA)
|   256 95:89:a4:12:e2:e6:ab:90:5d:45:19:ff:41:5f:74:ce (ECDSA)
|_  256 e1:2a:96:a4:ea:8f:68:8f:cc:74:b8:f0:28:72:70:cd (ED25519)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=12/5%OT=21%CT=1%CU=30534%PV=Y%DS=2%DC=T%G=Y%TM=6751
OS:75D6%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=103%TI=Z%CI=Z%II=I%TS=A)
OS:SEQ(SP=103%GCD=1%ISR=104%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=104%GCD=1%ISR=104%TI
OS:=Z%CI=Z%II=I%TS=9)SEQ(SP=105%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=10
OS:5%GCD=2%ISR=105%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3
OS:=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F
OS:4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW
OS:7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF
OS:=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=
OS:%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=
OS:0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RI
OS:PCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: ANONYMOUS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: anonymous
|   NetBIOS computer name: ANONYMOUS\x00
|   Domain name: \x00
|   FQDN: anonymous
|_  System time: 2024-12-05T09:43:49+00:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-12-05T09:43:49
|_  start_date: N/A

TRACEROUTE (using port 3306/tcp)
HOP RTT      ADDRESS
1   29.56 ms 10.9.0.1
2   29.59 ms 10.10.42.241

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Dec  5 10:43:50 2024 -- 1 IP address (1 host up) scanned in 37.07 seconds

```

Nous trouvons un **FTP anonymous** en 21 et la présence du **service SMB** en 139/245

### 2. SMB

```
smbclient -L $IP
```



### 3. Réponses aux questions

<table><thead><tr><th width="495">Questions</th><th>Réponses</th></tr></thead><tbody><tr><td>Enumerate the machine. How many ports are open?</td><td>4</td></tr><tr><td>What service is running on port 21?</td><td>FTP</td></tr><tr><td>What service is running on ports 139 and 445?</td><td>SMB</td></tr><tr><td>There's a share on the user's computer.  What's it called?</td><td>pics</td></tr></tbody></table>




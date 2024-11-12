---
description: Challenge niveau medium du site TryHackMe
icon: caret-right
---

# \[GoldenEye]\[CTF]\[TryHackMe]

Page de réserve pour le challenge \[GoldenEye]

<figure><img src=".gitbook/assets/thm-thunder-02 (1).png" alt=""><figcaption><p>Tumbnail [GoldenEye]</p></figcaption></figure>

> "_This room will be a guided challenge to hack the James Bond styled box and get root. Credit to creosote for creating this VM."_

[Lien vers Tryhackme.com](https://tryhackme.com/r/room/goldeneye)

## **\[Task 1] Intro & Enumeration**

Commençons les énumérations de façon classique avec Nmap et Gobuster.

### [Nmap](outils.md#nmap)

```sh
nmap $IP -A -p- -oN nmap.txt -T4
```

```shell-session
PORT      STATE SERVICE     VERSION
25/tcp    open  smtp        Postfix smtpd
|_smtp-commands: ubuntu, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
|_ssl-date: TLS randomness does not represent time
80/tcp    open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: GoldenEye Primary Admin Server
55006/tcp open  ssl/unknown
|_ssl-date: TLS randomness does not represent time
55007/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: TOP PIPELINING UIDL STLS USER SASL(PLAIN) AUTH-RESP-CODE CAPA RESP-CODES
|_ssl-date: TLS randomness does not represent time
```

Ce qui nous donne déjà une première réponse :

<table><thead><tr><th width="626">Question</th><th>Réponse</th></tr></thead><tbody><tr><td>Use nmap to scan the network for all ports. How many ports are open?</td><td>4</td></tr></tbody></table>

### [Gobuster](outils.md#gobuster)

```sh
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt" -u $IP -t50
```

```
┌──(kali㉿kali)-[~/THM/goldeneye]
└─$ gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt" -u $IP -t50
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.2.201
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 282]
/.html                (Status: 403) [Size: 283]
/index.html           (Status: 200) [Size: 252]
/.php                 (Status: 403) [Size: 282]
/.html                (Status: 403) [Size: 283]
/server-status        (Status: 403) [Size: 291]
Progress: 882240 / 882244 (100.00%)
===============================================================
Finished
===============================================================

```

### Exploitation

Rendons-nous sur la page html du site avec Firefox

<figure><img src=".gitbook/assets/première-page.png" alt=""><figcaption><p>première page</p></figcaption></figure>

analysons le code source de la page

<figure><img src=".gitbook/assets/index-source.png" alt=""><figcaption><p>code source première page</p></figcaption></figure>

comme recommandé dans le challenge, inspectons tous les scripts et en particulier **terminal.js**

### code source de terminal.js

```javascript
var data = [
  {
    GoldenEyeText: "<span><br/>Severnaya Auxiliary Control Station<br/>****TOP SECRET ACCESS****<br/>Accessing Server Identity<br/>Server Name:....................<br/>GOLDENEYE<br/><br/>User: UNKNOWN<br/><span>Naviagate to /sev-home/ to login</span>"
  }
];

//
//Boris, make sure you update your default password. 
//My sources say MI6 maybe planning to infiltrate. 
//Be on the lookout for any suspicious network traffic....
//
//I encoded you p@ssword below...
//
//&#73;&#110;&#118;&#105;&#110;&#99;&#105;&#98;&#108;&#101;&#72;&#97;&#99;&#107;&#51;&#114;
//
//BTW Natalya says she can break your codes
//

var allElements = document.getElementsByClassName("typeing");
for (var j = 0; j < allElements.length; j++) {
  var currentElementId = allElements[j].id;
  var currentElementIdContent = data[0][currentElementId];
  var element = document.getElementById(currentElementId);
  var devTypeText = currentElementIdContent;

 
  var i = 0, isTag, text;
  (function type() {
    text = devTypeText.slice(0, ++i);
    if (text === devTypeText) return;
    element.innerHTML = text + `<span class='blinker'>&#32;</span>`;
    var char = text.slice(-1);
    if (char === "<") isTag = true;
    if (char === ">") isTag = false;
    if (isTag) return type();
    setTimeout(type, 60);
  })();
}
```

<table><thead><tr><th width="574">Question</th><th>Réponse</th></tr></thead><tbody><tr><td>Who needs to make sure they update their default password?</td><td>boris</td></tr></tbody></table>

### Cyber Chef

Le code source nous révèle un hash qui ressemble furieusement à des codes ASCII.

```
&#73;&#110;&#118;&#105;&#110;&#99;&#105;&#98;&#108;&#101;&#72;&#97;&#99;&#107;&#51;&#114;
```

Confions ce hash à [cyber chef](https://gchq.github.io/CyberChef/) et au passage, remarquons qu'il nous propose le décodage « magique »

<figure><img src=".gitbook/assets/cyberchef01.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/cyberchef02.png" alt=""><figcaption><p>cyberchef</p></figcaption></figure>

Ce qui nous donne la réponse à la question suivante.

<table><thead><tr><th width="505">Question</th><th>Réponse</th></tr></thead><tbody><tr><td>Whats their password?</td><td>InvincibleHack3r</td></tr></tbody></table>

**Nous connaissons maintenant deux utilisateurs, boris et natalya et nous avons un mot de passe pour boris.**

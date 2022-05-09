+++
author = "Jonathan Serra"
title = "Installer un nœud Lightning Network"
date = "2019-12-31"
description = "Envoyer des microtransactions bitcoin n'était pas possible jusqu'à l'arrivée de la technologie Lightning Network. C'est un système qui permet de faire des transactions quasi-instantannées et avec peu de frais."
tags = [
  "bitcoin", "lightning network", "debian", "bitcoin-cli"
]
image = "images/thumbnails/lnd.jpg"
+++

## Matériels requis

Avant tout il va vous falloir du matériel puisqu'un nœud est un serveur qui fourni un service. Ne vous inquiétez pas vous n'aurez pas besoin d'un serveur coûteux, bruyant et consommateur. Vous aurez besoin d'un nano-ordinateur, c'est petit, c'est silencieux et la consommation est basse, voilà la liste :
- **Raspberry Pi 3** : [ce matériel](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/) est peu coûteux et a les capacités optimales pour avoir un nœud LN efficace. Veillez à acquérir un pack contenant le Raspberry Pi, l'alimentation secteur et une carte SSD. Ces éléments sont compris dans le pack officiel ;
- **Un disque dur SSD de 500go ou plus** : vous devrez télécharger la blockchain de Bitcoin qui fait plus de 200 giga octects. Je recommande un disque dur SSD (électronique) à préférer au disque dur mécanique qui a une moins bonne tolérance à la panne. J'ai personnellement un disque dur mécanique ;
- **Un écran port HDMI** : ce sera nécessaire pour faire les premières configurations ;
- **Un clavier USB** : ce sera également nécessaire pour faire les premières configurations ;
- **Un câble RJ45** : ce sera pour connecter votre Raspberry Pi à Internet. Il est possible d'avoir une connexion wifi en y connectant une antenne ou un dongle. Cependant je déconseille cette approche pour des raisons de sécurité et également pour éviter les contraintes d'installation de drivers pas toujours disponibles sur les distributions Linux ;

## Installer Raspian Stretch Lite

_Cette étape est optionnelle mais recommandée._

Avant d'entamer la configuration de votre Raspberry installez un nouveau système d'exploitation plus léger que celui installé par défaut : c'est [Raspian Stretch Lite](https://www.raspberrypi.org/downloads/raspbian/).

> Stretch Lite est un système d'exploitation dans le but d'optimiser la consommation de ressources sur notre système.

Installez Raspian Stretch Lite sur la carte SD fournie avec le Raspberry Pi. Il vous faudra la formater. Avec Windows le logiciel [Etcher](https://www.balena.io/etcher/) est d'une grande aide. Sur [Mac](https://computers.tutsplus.com/articles/how-to-flash-an-sd-card-for-raspberry-pi--mac-53600) et [distributions Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md) préférez la ligne de commande.

## Configurer le Raspberry Pi

Ici je vais considérer que vous êtes sur Raspian Stretch Lite, mais pas d'inquiétude vous pouvez tout suivre avec le système d'origine, veillez à ouvrir le terminal car nous allons travailler en ligne de commande, rien de mieux pour se familiariser avec l'environnement Linux.

Les accès par défaut sont les suivants

{{< highlight shell >}}
raspberry login: pi
Password: raspberry
{{</ highlight >}}

> Si vous n'êtes pas habitué au terminal Debian, en tapant votre mot de passe rien ne s'affiche, c'est tout à faire normal, ce que vous tapez est quand même pris en compte. C'est une protection au même titre que les étoiles qu'on voit sur les formulaires web.

### Activer et configurer SSH

Actuellement vous travaillez avec un écran et un clavier branchés sur votre petit Rapsberry... Et c'est peut-être pas pratique. J'ai une bonne nouvelle, on va pouvoir faire en sorte de gérer votre système sans avoir besoin d'y accéder physiquement. Vous allez le contrôler à distance grâce au protocole SSH !

> SSH signifie Secure Shell, c'est un protocole de réseau ingénieux qui permet l'exécution de commandes à distance en bénéficiant de canaux cryptés. Il est possible d'utiliser SSH au sein d'un réseau local mais aussi en passant par Internet par exemple. Sachez que par défaut la configuration du SSH dans Raspian permet l'authentification par login et mot de passe.

Par défaut la connexion SSH est désactivée pour des raisons de sécurité, alors activez le en tapant ce qui suit

{{< highlight shell >}}
 pi@raspberrypi:~ $ sudo raspi-config
{{</ highlight >}}

Un menu graphique peu charmant s'ouvre, sélectionnez `Interfacing Options (Configure connections to peripherals)` puis `ssh (Enable or disable ssh server)` ensuite sélectionnez `Yes`, `OK` et enfin `Finish`.

Revenez sur le terminal et redémarrez le service ssh

{{< highlight shell >}}
 pi@raspberrypi:~ $ sudo systemctl restart ssh
{{</ highlight >}}

Votre SSH est maintenant activé.

À la bonheur, vous pouvez dès maintenant vous connecter à votre Raspberry à distance, depuis un autre ordinateur. D'abord récupérez l'adresse IP de votre Raspberry vous en aurez besoin, utilisez la commande `ifconfig` pour l'afficher. Voilà un extrait de mon `ifconfig`

{{< highlight shell >}}
pi@raspberrypi:~ $ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>;  mtu 1500
        inet 192.168.1.4 netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::10e0:8a76:4fe4:c3c2  prefixlen 64  scopeid 0x20
        ether b8:27:eb:6a:3a:d9  txqueuelen 1000  (Ethernet)
        RX packets 169483183  bytes 168117311599 (156.5 GiB)
        RX errors 2  dropped 45841  overruns 0  frame 0
        TX packets 132373249  bytes 17426498366 (16.2 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>;  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 39066126  bytes 154986068081 (144.3 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 39066126  bytes 154986068081 (144.3 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4099<UP,BROADCAST,MULTICAST>;  mtu 1500
        ether b8:27:eb:3f:6f:8c  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
{{< /highlight >}}

Voyez en surligné mon adresse IP est `192.168.1.42` — puisque c'est la réponse à tous les mystères de l'univers. Vous la trouverez normalement sur le connecteur `eth0` et en `ipv4` qui est le format que j'ai sélectionné et que je vous recommande pour éviter des configurations futures.

Avec votre ordinateur favori, que ce soit sur MacOS, Windows, Ubuntu, Trisquel ou TempleOS vous pouvez utiliser un outil qui permet de vous connecter à votre Raspberry avec SSH. Le plus simple avec MacOS ou une distribution Linux. Si vous n'avez pas `ssh` de disponible en commande, veuillez installer [OpenSSH](https://www.openssh.com/). Voilà comment procéder avec votre terminal sachant que mon IP est `192.168.1.42`

{{< highlight shell >}}
monpc@oumonmac:~ $ ssh-keygen -t rsa -b 4096
>; Generating public/private rsa key pair.
>; Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter] Laissez par défaut
>; Enter passphrase (empty for no passphrase): [Type a passphrase] Votre mot de passe
>; Enter same passphrase again: [Type passphrase again] Confirmez votre mot de passe

monpc@oumonmac:~ $ ssh pi@192.168.1.42
pi@192.168.1.42's password: Entrez votre mot de passe
Linux raspberrypi 4.14.71-v7+ #1145 SMP Fri Sep 21 15:38:35 BST 2018 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/* /copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Mar 19 02:04:32 2019 from 192.168.1.44

pi@raspberrypi:~ &
{{</ highlight >}}

Cette étape permet d'avoir une clé RSA pour initialiser une communication chiffrée avec votre Raspberry et de connecter ensuite.

Il existe des solutions avec interface comme [PuTTY](https://www.putty.org/) pour Windows. Vous devrez entrer l'adresse IP de votre Raspberry, le login `pi` et le mot de passe `raspberry`.

### Créer un utilisateur

Afin de protéger votre installation du mieux que possible je recommande de créer un utilisateur prévu à cet usage. Nous l'appellerons "bitcoin".

{{< highlight shell >}}
pi@raspberrypi:~ $ adduser bitcoin
Adding user 'bitcoin' ...
Adding new group 'bitcoin' (1001) ...
Adding new user 'bitcoin' (1001) with group 'bitcoin' ...
Creating home directory '/home/bitcoin' ...
Copying files from '/etc/skel' ...
Enter new UNIX password: Mettez un mot de passe fort
Retype new UNIX password: Confirmez votre mot de passe
passwd: password updated successfully
Changing the user information for bitcoin
Enter the new value, or press ENTER for the default
&nbsp;&nbsp;Full Name []: Vous pouvez laisser tous ces champs vides
&nbsp;&nbsp;Room Number []:
&nbsp;&nbsp;Work Phone []:
&nbsp;&nbsp;Home Phone []:
&nbsp;&nbsp;Other []:
Is the information correct? [Y/n] Y
{{</ highlight >}}

> Vous pouvez désormais vous connecter directement à l'utilisateur "bitcoin" avec SSH en tapant par exemple `ssh bitcoin@192.168.1.42`

## Préparer le disque dur

Vous aurez besoin de télécharger l'entièreté de la blockchain Bitcoin qui fait [250go](https://bitinfocharts.com/bitcoin/) à l'heure où j'écris ces lignes. Cela signifie que la petite carte SD du Raspberry ne peut pas mémoriser tout ça.    On va faire en sorte que toutes les informations relatives à la blockchain résident dans le disque externe.

Une fois que vous avez branché votre disque dur sur votre Raspberry, vous devriez le retrouver en tapant la commande `df -h`. Cette commande va vous lister quelque chose qui ressemble à ce qui suit :

{{< highlight shell >}}
pi@raspberrypi:~ $ df -h
/dev/root        15G  4.2G  9.8G  30%
devtmpfs        460M     0  460M   0% /dev
tmpfs           464M     0  464M   0% /dev/shm
tmpfs           464M  6.2M  458M   2% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           464M     0  464M   0% /sys/fs/cgroup
/dev/mmcblk0p1   44M   22M   22M  51% /boot
Highlight>/dev/sda        458G  249G  186G  58% /home/bitcoin/.bitcoi</ Highlight >tmpfs            93M     0   93M   0% /run/user/1001
{{</ highlight >}}

Vous devrez trouver un élément ayant la même capacité de stockage que votre disque. Mon disque fait 500go et on le trouve sur le chemin `/dev/sda`. En général on le devine dans la mesure où c'est le seul disque ayant cette capacité. Notez ce nom quelque part, et retenez cette commande `df` qui est pratique pour gérer ses disques.

### Formater (optionnel)

Votre disque dur doit maintenant être vierge (à moins que vous y avez enregistré la blockchain, dans ce cas ignorez cette étape). Tapez la commande `fdisk [votre disque]` où [votre disque] devient le nom de votre disque, dans mon exemple c'est `fdisk /dev/sda`.

### Monter le disque

Enfin il faut monter le disque sur le dossier de travail de Bitcoin, on va lui préparer sa maison et son nid douillet. Monter un disque signifie que nous allons associer un répertoire à un disque, octroyant une capacité de mémoire certaine à un répertoire. Et pour ce faire connectez vous à l'utilisateur que nous avions précédemment créé `su bitcoin`, puis tapez `mkdir .bitcoin .lnd`.

Il ne vous reste plus qu'à taper `mount <votre disque> .bitcoin` où votre disque est le chemin trouvé avec `df`.

> mkdir signifie make directory, c'est pour créer un dossier et .bitcoin est le nom du dossier. Par défaut nous travaillerons avec ce dossier pour la blockchain et les portefeuilles. Le "." est un moyen de cacher le dossier, cependant en aucun cas cela constitue sécurité suffisante.

## Installer bitcoind et bitcoin-cli

_Pour cette partie restez connecté à l'utilisateur `bitcoin` en tapant `su bitcoin`._

`bitcoind` est le système central de Bitcoin, le "d" à la fin signifie "daemon" qui désigne un service exécuté et maintenu par la machine. `bitcoin-cli` est une interface de commande qui permet d’interagir avec le service Bitcoin. Les deux s'installent ensemble, ce qui est super !

D'abord vous devrez chercher les fichiers d'installation de Bitcoin, je vous le donne ici : [bitcoin-0.19.0.1-arm-linux-gnueabihf.tar.gz](https://bitcoin.org/bin/bitcoin-core-0.19.0.1/bitcoin-0.19.0.1-arm-linux-gnueabihf.tar.gz). Gardez ce lien vous en aurez besoin.

Je tiens à souligner que vous pouvez le chercher par vous même en vous rendant [ici](https://bitcoincore.org/en/download/) et en sélectionnant "ARM Linux 64bits". Je vous conseille de le faire vous même car les versions sont mises à jour, ci-dessus vous avez la version 0.19.0.1 de Bitcoin Core.

Suivez les commandes ci-dessous pour installer `bitcoind`

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ wget https://bitcoin.org/bin/bitcoin-core-0.19.0.1/bitcoin-0.19.0.1-arm-linux-gnueabihf.tar.gz
bitcoin@raspberrypi:~ $ tar -vxf bitcoin-0.19.0.1-arm-linux-gnueabihf.tar.gz
bitcoin@raspberrypi:~ $ sudo install -m 0755 -o bitcoin -g bitcoin -t /usr/local/bin bitcoin-0.19.0.1/bin/*
bitcoin@raspberrypi:~ $ rm -rf bitcoin-0.19.0.1
{{</ highlight >}}

Notez qu'on donne les droits uniquement à l'utilisateur `bitcoin`. Désormais tapez les commandes `bitcoind -version` et `bitcoin-cli -version`. Si tout est bien installé vous verrez les versions s'afficher.

## Démarrer bitcoind

`bitcoind` est installé, il reste plus qu'à le démarrer pour qu'il puisse télécharger frénétiquement la blockchain en se connectant à des nœuds du réseau; automatiquement.

D'abord je vous recommande de vérifier si vous avez bien connecté votre disque dur, suivez l'exemple ci-dessous

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT"
sda           8:0    0 465.8G  0 disk /home/bitcoin/.bitcoin
mmcblk0     179:0    0  14.9G  0 disk
  |-mmcblk0p1 179:1    0  43.8M  0 part /boot
    -mmcblk0p2 179:2    0  14.8G  0 part /
{{</ highlight >}}

On peut voir en surligné que `/home/bitcoin/.bitcoin` est bien connecté au disque ayant 465.8Go d'espace. Si vous n'avez pas cela, revenez sur la partie [Monter le disque](#monter-le-disque).

Maintenant on peut écrire le fichier de configuration Bitcoin. Tapez `nano .bitcoin/bitcoin.conf`, cela va vous ouvrir un éditeur directement dans le terminal, ce qui vous permet d'écrire les configurations. Voilà des configurations que je recommande :

{{< highlight shell >}}
server=1
daemon=1
txindex=1

rpcuser=<Mettez un nom d'utilisateur>
rpcpassword=<Mettez un mot de passe>

onlynet=ipv4
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubrawtx=tcp://127.0.0.1:28333

dbcache=50
maxorphantx=10
maxmempool=50
maxconnections=40
maxuploadtarget=1000
{{< /highlight >}}

Pour sauvegarder tapez CTRL + O et CTRL + X pour quitter.

Et enfin tapez `sudo bitcoind` qui aura pour effet de lancer le service Bitcoin qui va rapidement commencer à télécharger la blockchain. Pour rappel cette dernière fait près de 200Go donc vous pouvez prendre une pause ici.

Pour observer l'avancement tapez la commande `tail -f .bitcoin/debug.log` vous verrez des entrées qui ressemblent à ce qui suit

```
2019-04-15T22:54:29Z UpdateTip: new best=00000000000000000024f0048c611086130b387e6634d8e2617c261a0c44b6dd height=571800 version=0x20c00000 log2_work=90.545983 tx=402724776 date='2019-04-15T22:54:19Z' progress=1.000000 cache=14.5MiB(103309txo) warning='35 of last 100 blocks have unexpected version'
```

L'information intéressante ici est particulièrement `progress=1.000000`. Cette valeur est en réalité la progression de la synchronisation de la blockchain, 1 signifie 100%, si vous avez par exemple `progress=0.014` alors vous en êtes à 1.4%. Pour quitter le `tail` tapez CTRL + C.

## Installer LND et LnCLI

_Pour cette partie restez connecté à l'utilisateur `bitcoin` en tapant `su bitcoin`._

`lnd` est le service qui va transformer le RaspberryPi en un nœud Lightning Network. Il va se connecter automatiquement a l'interface de commande du service `bitcoind`. Tout comme `bitcoind` LND embarque l'interface de commande `lncli`.

La procédure pour installer LND ressemble à l'installation de Bitcoin. La première chose à faire est de télécharger le service en vous rendant sur ce lien [github.com/lightningnetwork/lnd/releases](https://github.com/lightningnetwork/lnd/releases). Ici vous trouverez toutes les versions officielles de LND. Récupérez maintenant le lien de la dernière version "linux-armv7", voici celle que j'ai à l'heure où j'écris cet article [lnd-linux-armv7-v0.8.2-beta.tar.gz](https://github.com/lightningnetwork/lnd/releases/download/v0.8.2-beta/lnd-linux-armv7-v0.8.2-beta.tar.gz).

Notez bien que mon exemple s'applique pour la version que j'ai envoyée : v0.8.2beta.

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ wget https://github.com/lightningnetwork/lnd/releases/download/v0.8.2-beta/lnd-linux-armv7-v0.8.2-beta.tar.gz
bitcoin@raspberrypi:~ $ tar -vxf lnd-linux-armv7-v0.8.2-beta.tar.gz
bitcoin@raspberrypi:~ $ sudo install -m 0755 -o bitcoin -g bitcoin -t /user/local/bin lnd-linux-armv7-v0.8.2-beta/*
bitcoin@raspberrypi:~ $ rm -rf lnd-linux-armv7-v0.8.2-beta
{{</ highlight >}}

Désormais tapez la commande `lnd -version`. Si tout est bien installé vous verrez la version s'afficher.

Avant de démarrer LND créez son fichier de configuration en tapant `nano .lnd/lnd.conf`. Trouvez ci-dessous une configuration que je propose, vous pouvez évidemment mettre ce que vous voulez pour certaines options.

{{< highlight shell >}}
[Application Options]
debuglevel=info
maxpendingchannels=3
alias=<Votre Alias / pseudonyme / nom / ...>
color=<Une couleur en hexadécimal, par exemple #F00 pour Rouge vif>

nat=true # Si vous vous connectez depuis une box Internet

[Bitcoin]
bitcoin.active=1

bitcoin.mainnet=1
bitcoin.node=bitcoind

[bitcoind]
bitcoind.rpcuser=<Ce que vous aviez mis dans .bitcoin/bitcoin.conf rpcuser=>
bitcoind.rpcpass=<Ce que vous aviez mis dans .bitcoin/bitcoin.conf rpcpassword=>
bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332
bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333

[autopilot]
autopilot.active=1
autopilot.maxchannels=3
autopilot.allocation=0.5
{{< /highlight >}}

> "autopilot" vous intrigue peut-être. C'est un algorithme qui connecte automatiquement des nœuds présents dans le réseau décentralisé, typiquement des nœuds Bitcoin et Lightning Network. Autopilot intègre le modèle [Barabási–Albert](https://en.wikipedia.org/wiki/Barab%C3%A1si%E2%80%93Albert_model) dans l'idée de produire un réseau pair-à-pair en théorie homogène. Personnellement je l'active mais en aucun cas cela est nécessaire.

## Démarrer LND

Tapez simplement `lnd` et voilà ! Votre nœud LN est en route. Il va d'abord synchroniser sa blockchain locale avec bitcoind. En attendant vous allez créer votre portefeuille. En effet LND ne va pas exploiter le portefeuille de bitcoind mais gérer le sien, cela veut dire que vous aurez un portefeuille relatif à votre nœud LN.

Laissez `lnd` tourner, ne quittez pas en faisant CTRL + C, je vous suggère d'ouvrir un nouveau terminal et vous connecter à nouveau sur `bitcoin`. Quitter votre terminal ne stoppera pas LND.

Une fois de retour suivez cette procédure ci-dessous :

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ lncli create
Input wallet password : <Mettez un mot de passe fort et écrivez le sur une feuille pour ne pas le perdre>
Confirm wallet password: <Confirmez votre mot de passe>

Do you have an existing cipher seed mnemonic you want to user? (Enter y/n): n

Your cipher seed can optionally be encrypted.
Input your passphrase if you wish to encrypt it (or press enter to proceed without a cipher seed passphrase): <Mettez un mot de passe ou pressez directement ENTREE>

Generating fresh cipher seed...

!!!YOU MUST WRITE DOWN THIS SEED TO BE ABLE TO RESTORE THE WALLET!!!

> Ici lncli affiche votre seed, notez la consciencieusement sur une feuille, dans l'ordre. Cette seed (graine) est la clé de votre portefeuille; elle vous assurera de ne jamais perdre vos fonds

!!!YOU MUST WRITE DOWN THIS SEED TO BE ABLE TO RESTORE THE WALLET!!!

lnd successfully initialized!
{{</ highlight >}}

TADAM ! Vous avez maintenant un tout nouveau portefeuille et LND sera capable de fonctionner... Après avoir tapé ces commandes

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ lncli unlock
bitcoin@raspberrypi:~ $ lncli newaddress p2wkh
{{< /highlight >}}

> "lncli unlock" est important pour que LND puisse fonctionner car il aura besoin d'un accès à votre portefeuille, ce dernier est chiffré par défaut et il faut le déchiffrer en le déverrouillant. Le mot de passe à insérer est le premier mot de passe que vous avez mis lors de la création du portefeuille avec `lncli create`

LND est maintenant prêt à fonctionner. Pour observer ce qu'il se passe tapez la commande `tail -f .lnd/debug.log`.

Pour vérifier que votre nœud est bien recensé sur les internets allez chercher votre clé publique en tapant `lncli getinfo` comme dans le terminal ci-dessous

{{< highlight shell >}}
bitcoin@raspberrypi:~ $ lncli getinfo
{
  "dentity_pubkey": "03a2c34daf010b3501daf704b5a321e82e1631421b7ffa18dd49014967eda82dc",
  "alias": "Blocs",
  "num_pending_channels": 0,
  "num_active_channels": 3,
  "num_peers": 4,
  "block_height": 572660,
  "block_hash": "0000000000000000001d7bf15a13ffb76a8c1256e5ab778ffcc918d97944e45a",
  "synced_to_chain": true,
  "testnet": false,
  "chains": [ "bitcoin" ],
  "uris": [
    "03a2c34daf010b3501daf704b5a321e82e1631421b7ffa18dd49014967eda82dc9@109.18.104.32:9735"
  ],
  "best_header_timestamp": "1555878831",
  "version": "0.5.2-beta commit=v0.5.2-beta",
  "num_inactive_channels": 0'
}
{{< /highlight >}}

Ma clé publique est `03a2c34daf010b3501daf704b5a321e82e1631421b7ffa18dd49014967eda82dc9`, elle sert d'identifiant au nœud. Maintenant si votre nœud est bien configuré et en route, vous pouvez le trouver sur ce site : [1ml.com](https://1ml.com) en tapant dans la recherche la clé publique de votre nœud.

---

Vous êtes maintenant propriétaire d'un nœud Bitcoin et d'un nœud Lightning Network. Cela signifie que vous avez la capacité d'utiliser n'importe quel portefeuille supportant LN en passant par votre propre serveur. Félicitations et bienvenue dans la communauté de l'a... Bitcoin et LN. Dans un article suivant on va voir comment avoir une interface graphique pour gérer son nœud sans passer par les lignes de commande comme nous l'avons fait tout au long de l'installation. Pour l'heure vous pouvez découvrir les possibilités de LN en tapant `lncli --help` et les possibilités de Bitcoin en tapant `bitcoin-cli --help`.

+++
author = "Jonathan Serra"
title = "Bitcoin SCRIPT en vidéo"
date = "2022-09-04"
description = "Lorsque nous payons avec Bitcoin nous signons un contrat. Plus spécifiquement nous produisons une signature cryptographique pour un programme qui fait office de contrat : c'est ce qui a été nommé Contrat Intelligent par Nick Szabo en 1997, 11 ans avant que le concept Bitcoin émerge."
tags = [
  "bitcoin", "script", "op code", "checklocktimeverify"
]
image = "images/thumbnails/tor_bitcoind_lnd.png"
draft = true
+++

Lorsque nous payons avec Bitcoin nous signons un contrat. Plus spécifiquement nous produisons une signature cryptographique pour un programme qui fait office de contrat : c'est ce qui a été nommé ["Contrat Intelligent"](https://nakamotoinstitute.org/formalizing-securing-relationships/) par [Nick Szabo](http://unenumerated.blogspot.com/) en 1997, 11 ans avant que le concept Bitcoin émerge.

L'idée principielle du contrat intelligent tend à répondre à trois qualités fondamentales d'un contrat _(ce que l'on a pour habitude de signer avec notre plus belle esquisse)_ par l'usage de solutions digitales :
- __Observabilité par principe__ : un contrat doit avoir une histoire que l'on doit connaître, sa date de création, son expriation éventuelle, le ou les parti(s) engagés, etc.
- __Vérifiable par une tierce partie__ : il doit être possible de vérifier l'authenticité d'un contrat ainsi que l'authenticité des signatures.
- __Effet relatif du contrat__ : un contrat ne doit engager que les partis concernés par ce dernier. Aucun tiers ne peut être engagé par un contrat.

En 1997 il n'y avait pas de solution parfaite pour permettre le contrat intelligent. On avait déjà les signatures cryptographiques qui permettaient une vérifiabilité. On avait les outils de télécommunication pour l'observabilité. Mais il n'existait rien qui combinait les différents outils pour avoir un service totalement digital répondant aux fondamentaux d'un contrat.

C'était en 1997, nous sommes en 2022 à l'heure où j'écris cet article, et il y a 14 ans une solution devient la clé de la boîte de Pandore du contrat intelligent _(c'est Bitcoin bien évidemment)_.

## Bitcoin, une chose protéiforme ?

Cet article va parler d'un sujet technique en vue de préciser un élément central à Bitcoin _(et les techno. Blockchain)_, mais en préambule je vais m'arrêter sur ce qu'est Bitcoin, au fond.

On entend ça et là que Bitcoin est une cryptomonnaie, oui, possible. On entend que Bitcoin est une Blockchain, éventuellement, dépend de ce qu'on veut dire. On entend que Bitcoin est un protocole pair-à-pair (P2P), certainement. C'est un peu de tout ça. De la même façon que l'humain n'est ni une tête, ni deux bras, ni deux jambes _(mais bien un gallinacé sans plumes)_.

Si Bitcoin est qualifié de (crypto)monnaie, c'est parce qu'il le permet. Si Bitcoin est qualifié de Blockchain, c'est parce qu'il en a besoin et le produit. Si Bitcoin est qualifié de protocole, c'est parce qu'il y en a un.

Finalement, c'est une chose à l'apparence protéiforme. Dire ce qu'est Bitcoin n'est foncièrement pas l'exercice le plus simple si la volonté est d'apporter une information complète _(et ni le plus intéressant)_. C'est ce que ça permet qui compte, et ce que ça permet : ce sont les contrats intelligents.

L'objet de cet article c'est de voir la tronche qu'ont ces fameux contrats, c'est juste en dessous !

> Vous aurez bien cerné qu'ici nous allons parler du corps des contrats, ce qu'ils sont matériellement. Ce qu'ils sont mécaniquement nécessite de parler d'autres parties qui composent Bitcoin, en l'occurence la preuve de travail, les transactions et leur timestamp, le protocole P2P, etc.

## Initiation aux contrats intelligents

Un contrat intelligent sur Bitcoin est composé de plusieurs parties.

__Prenons le temps de souligner que ce que nous allons voir concerne Bitcoin mais que le concept de ces objets est récurrent dans nombreuses autres technologies Blockchain comme Ethereum.__

### Au commencement était le verbe : Le SCRIPT et les OP CODE

Un contrat tacite est inscrit, et pour l'inscrire il faut une langue. Sur Bitcoin la langue utilisée est le `SCRIPT`. Les phrases en `SCRIPT` sont construites avec des `OP CODE`.

Le `SCRIPT` est donc un groupe d'opérations empilées et interprétées de haut en bas, c'est ce qui consistue _in fine_ le contrat intelligent.

Mettons, il existe un `OP CODE` d'addition appelé `OP_ADD`, un `SCRIPT` permettant d'additionner `4 + 2` ressemblerait à ceci :

```
SCRIPT:
4
2
OP_ADD
```

La pile est interprétée de haut en bas. La première valeur est `4`, c'est ce qu'on appelle une valeur constante, ce n'est pas un `OP_CODE`. Cette valeur constante est mémorisée dans une autre pile `MEMOIRE` :

```
SCRIPT:  |  MEMOIRE:
2        |  4
OP_ADD   |
```

Derechef, la valeur constante `2` est interprétée :

```
SCRIPT:  |  MEMOIRE:
OP_ADD   |  4
         |  2
```

Et enfin, le premier `OP_CODE` de la pile, l'addition. C'est un code d'opération qui a un comportement défini par le protocole. En l'occurence `OP_ADD` prend les 2 valeurs dernièrement ajoutées à la pile `MEMOIRE`, ce qui nous donne :

```
SCRIPT:  |  MEMOIRE:
         |  6
```

La valeur finale est ce qu'on appelle communément l'`OUTPUT` (la sortie). Et la pile `SCRIPT` est dénommée `INPUT` (l'entrée).

> Le concept du `OP CODE` (code opération) est apparu avec l'émergence des processeurs. Les processeurs prennent en entrée des valeurs binaires (des 1 ou des 0). Regroupées par 8 ces valeurs binaires forment des octets (ou bytes). Un `OP CODE` est un octet particulier quit peut être utilisé pour décrire une opération particulier : c'est l'`OP CODE`. En programmation l'`OP CODE` permet de définir universellement des opérations au sein d'un protocole, car toutes les machines peuvent interpréter des octets.

C'est bien sympa tout ça, c'est plutôt archaïque, vous ne trouvez pas ? Je vous rassure, c'est bien le cas. C'est une logique qui est appliquée en bas niveau, au plus proche des processeurs des machines. Au plus c'est bas niveau, au plus c'est logiquement simple... Mais ça devient vite illisible pour l'humain adepte de la sémantique.

On n'a pas encore parlé de signature... Or, le contrat concerne tout de même une ou plusieurs parties signataires ! Il est temps de voir le `scriptSig` et le `scriptPubKey`.

### Signé c'est pesé : le scriptSig et le scriptPubKey

D'un point de vue purement `SCRIPT` le `scriptSig` et le `scriptPubKey` forment à eux deux un `SCRIPT`... Rien de nouveau.

Cependant, ces deux éléments amènent à s'intéresser à un concept du contrat intelligent qui dépasse le simple cadre du langage et son interprétation.

Pour revenir un peu sur le sujet d'un contrat, c'est un corpus qui implique un ou des parties. Un contrat est observé et vérifié par une tierce partie.

Dans le cadre d'un contrat intelligent, il peut être en suspens lorsqu'il n'est pas signé par un ou plusieurs partis : cela est régi par le script de verrouillage (ou _locking script_), c'est le `scriptPubKey`. Cette suspension est débloquée grâce au script de déverrouillage (ou _unlocking script_), c'est le `scriptSig`. Dans ce cadre, ce qui exécute le déverrouillage d'un contrat intelligent est le protocole Bitcoin _(si le `scriptSig` est valide évidemment, c'est tout le jeu de l'observabilité et de la vérification)_. Le protocole Bitcoin a alors le rôle de tierce parti arbitrant le contrat qui implique un ou des parties.

OK super... Mais c'est quoi au fond le `scriptSig` et le `scriptPubKey` ?

Le `scriptSig` peut être essentiellement n'importe quoi servant de clé à l'exécution du contrat intelligent. C'est aucune, une ou plusieurs valeur(s), qui sont directement ajoutées sur le dessus de la pile `INPUT` du SCRIPT.

Le `scriptPubKey` est le SCRIPT qui servira à déterminer si le `scriptSig` est valide ou non.

> On notera deux choses intéressantes. La première c'est qu'un `scriptSig` vide et valide signifie que le contrat nécessite de rien pour être valide. La seconde c'est qu'un `scriptPubKey` peut très bien être un SCRIPT qui sera toujours invalide peu importe le `scriptSig`, ce qui correspond à un contrat corrompu... Et cela peut avoir une utilité.

Pour illustrer tout cela, prenons un SCRIPT qui valide si une addition est juste, si elle est juste le contrat intelligent est alors valide.

```
scriptSig: 4 2
scriptPubKey: 6 OP_ADD OP_EQUALVERIFY

SCRIPT(INPUT):     MEMOIRE(OUTPUT):      
4               |
2               |
OP_ADD          |
6               |
OP_EQUALVERIFY  |
```

Le `scriptSig` est ajouté en haut de la pile `INPUT`, puis suit le `scriptPubKey`. `OP_EQUALVERIFY` est un nouvel OP CODE. Ce dernier va simplement vérifier sur les deux dernières valeurs de la pile `OUTPUT` sont égales.

Exécutons l'entièreté du `SCRIPT` :

```
cycle 1
--
SCRIPT(INPUT):     MEMOIRE(OUTPUT):
2               |  4
OP_ADD          |
6               |
OP_EQUALVERIFY  |


cycle 2:
--
SCRIPT(INPUT):     MEMOIRE(OUTPUT):
OP_ADD          |  4
6               |  2
OP_EQUALVERIFY  |


cycle 3:
--
SCRIPT(INPUT):     MEMOIRE(OUTPUT):
6               |  6
OP_EQUALVERIFY  |


cycle 4:
--
SCRIPT(INPUT):     MEMOIRE(OUTPUT):
OP_EQUALVERIFY  |  6
                |  6


cycle 5:
--
SCRIPT(INPUT):     MEMOIRE(OUTPUT):
                |  True
```

La dernière valeur est `True`, valeur usuelle comprise comme Vraie. Cela signifie que le `scriptSig = 4 2` vérifie bien le SCRIPT `6 OP_ADD OP_EQUALVERIFY`. Dit autrement `4 + 2` est bien égale à `6`.

Du coup `4 + 2` c'est une signature ? Et bien, je vais devoir avouer que le terme "intelligent" n'est peut-être pas le mot le plus idoine. Mais pour ça c'est Nick Szabo qu'il faut aller voir.

Hormis les cas bien particuliers, un contrat demande un `scriptSig` prouvant que la personne est l'unique personne étant capable de fournir la bonne valeur. C'est ici que se trouve la signature cryptographique. Nous allons le voir des exemples avec des cas réels ci-dessous.

## P2PK : Première version de paiement (obsolète)

Le P2PK fait parti des premiers SCRIPT Bitcoin existant sur Bitcoin Core. Sa fonction est de contrôler qu'un signataire est bien possesseur de la clé attendue dans le `scriptPubKey`.

Ce SCRIPT comporte un défaut - raison pour laquelle il est obsolète _(mais toujours actif)_. Ce dernier nécessite de faire connaître la clé publique du signataire attendu dans le `scriptPubKey`. C'est un défaut pouvant compromettre la sécurité, évidemment avec cette seule information le système reste sécurisé, cependant il existe des approches permettant de ne pas dévoiler la clé publique _(voir P2PKH juste en dessous)_.

{{< video src="videos/btc_opcode_p2pk.mp4" caption="Animation d'exécution de P2PK" >}}

```
scriptSig: <signature=304...0ef>
scriptPubKey: <pubKey=038...f7d> OP_CHECKSIG
```

>  On remarque que l'OP CODE `OP_CHECKSIG` est ce qui va contrôler la signature. Ce contrôle repose sur la signature fournie via `scriptSig` avec l'algorithme ECDSA (signature cryptographique sur courbe elliptique). Ce sujet mériterait des articles à part tant il y a de choses à en dire. Ce qu'on peut noter surtout, c'est que le principe de l'OP CODE offre une certaine modularité. Si un jour on voulait ajouter un nouvel algorithme pour signer les transactions, un nouvel OP CODE pourrait faire l'affaire. 

## P2PKH : The Paiement

P2PKH est probablement le SCRIPT le plus populaire. Il est le Super Cousin de P2PK, où le "H" ajoute une propriété de hachage afin de ne pas divulguer la clé publique du signataire dans le `scriptPubKey`. Voyez par vous-même et comparez le avec le P2PK, ce qui est lisible dans le `scriptPubKey` c'est le hash de la clé publique.

{{< video src="videos/btc_opcode_p2pkh.mp4" caption="Animation d'exécution de P2PKH" >}}

```
scriptSig: <signature=304...667> <pubKey=03e...ba7>
scriptPubKey: OP_DUP OP_HASH160 <pubKeyHash=bb2...e3e> OP_EQUALVERIFY OP_CHECKSIG
```

> `OP_HASH160` applique un hash avec [RIPEMD160](https://fr.wikipedia.org/wiki/RIPEMD-160)

## Gèle temporaire d'une transaction

Geler des fonds pour un temps donné ? Et oui c'est possible avec la carte... Bitcoin.

{{< video src="videos/btc_opcode_freeze.mp4" caption="Animation d'exécution de freeze de transaction" >}}

```
scriptSig: <signature=304...488> <pubKey=023...da2>
scriptPubKey: <expiryTime=2000> OP_CHECKLOCKTIMEVERIFY OP_DROP OP_DUP OP_HASH160 <pubKeyHash=909...5e8> OP_EQUALVERIFY OP_CHECKSIG
```

> `OP_CHECKLOCKTIMEVERIFY` est un OP CODE particulier. Cette opération va chercher une valeur disponible dans la transaction, c'est le `nLockTime`. Dit simplement, le `nLockTime` est une information spécifiant à quel moment la transaction peut être incluse dans un bloc, et donc inscrite dans la blockchain. Notez par ailleurs que quand bien même le `nLockTime` peut être une information de temps, le protocole ne peut jamais assurer que la transaction soit inscrite à ce temps donné. On doit considérer qu'elle le sera une fois que ce temps soit passé. Cela est dû au principe de la preuve de travail et de la validation moyenne des blocs toutes les 10 minutes.

## Puzzle Bitcoin

Gagnez des milliards si vous trouvez le mot secret. C'est le but de ce SCRIPT _(pas celui-ci, enfin il n'y a pas des milliards à remporter désolé)_.

{{< video src="videos/btc_opcode_puzzle.mp4" caption="Animation d'exécution d'un Puzzle Bitcoin" >}}

```
scriptSig: <data=satoshi>
scriptPubKey: OP_HASH256 <hash=da2...26f> OP_EQUAL
```

# Conclusion

Les OP CODE et le SCRIPT offrent pas mal de possibilités pour exploiter l'autonomie du réseau et l'exécution des SCRIPTs. Avec par exemple `OP_CHECKLOCKTIMEVERIFY` qui lit une information dans la transaction. En réalité c'est une partie hautement modulaire du protocole Bitcoin propice à l'évolution du protocole... Qui ne se fait jamais sans discussions autour des contributeurs de Bitcoin.

Nous avons ici uniquement la partie SCRIPT, sachez que c'est qu'un petit morceau de ce qu'est une transaction. Disons que nous venons de voir le cerveau de la transaction.

Les vidéos que vous voyez ont été générées avec [Manim](https://www.manim.community/). Le code source est disponible ici [https://github.com/Chaine-de-Blocs/bitcoin-manim](https://github.com/Chaine-de-Blocs/bitcoin-manim).
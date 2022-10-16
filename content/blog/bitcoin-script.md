+++
author = "Jonathan Serra"
title = "Bitcoin Script"
date = "2022-09-04"
description = "Vous avez un nœud Bitcoin et Lightning Network, vous participez à la décentralisation du réseau, bravo ! Cependant il réside un point noir, en particulier si votre noeud est localisé chez vous ou votre bureau... En connaissant votre adresse IP, publiée par le noeud, il est possible de vous localiser. On remédie à cela avec Tor."
tags = [
  "bitcoin", "debian", "bitcoin-cli", "tor"
]
image = "images/thumbnails/tor_bitcoind_lnd.png"
draft = true
+++

Lorsque nous payons avec Bitcoin nous signons un contrat. Plus spécifiquement nous produisons une signature cryptographique à un programme qui fait office de contrat : c'est ce qui a été nommé ["Contrat Intelligent"](https://nakamotoinstitute.org/formalizing-securing-relationships/) par [Nick Szabo](http://unenumerated.blogspot.com/) en 1997, 11 ans avant que le concept Bitcoin émerge.

L'idée principielle du contrat intelligent tend à répondre à trois qualités fondamentales d'un contrat (ce que l'on a pour habitude de signer avec notre plus belle esquisse) par l'usage de solutions digitales :
- __Observabilité par principe__ : un contrat doit avoir une histoire que l'on doit connaître, sa date de création, son expriation éventuelle, le ou les parti(s) engagés, etc.
- __Vérifiable par une tierce partie__ : il doit être possible de vérifier l'authenticité d'un contrat ainsi que l'authenticité des signatures (analyse graphologique pour la signature manuscrite par exemple).
- __Effet relatif du contrat__ : un contrat ne doit engager que les parties concernées par ce dernier. Aucun tiers ne peut être engagé par un contrat.

En 1997 il n'y avait pas de solution parfaite pour permettre le contrat intelligent. On avait déjà les signatures cryprographiques qui permettaient une vérifiabilité. On avait les outils de télécommunication pour l'observabilité. Mais il n'existait rien qui combinait les différents outils pour avoir un service totalement digital répondant aux fondamentaux d'un contrat.

C'était en 1997, nous sommes en 2022 à l'heure où j'écris cet article, et il y a 14 ans une solution devient la clé de la boîte de Pandore du contrat intelligent (c'est Bitcoin bien évidemment).

## Bitcoin, une chose protéiforme ?

Cet article va parler d'un sujet technique précisant un élément central à Bitcoin (et les techno. Blockchain), mais en préambule j'aimerai m'arrêter sur ce qu'est Bitcoin, au fond.

On entend ça et là que Bitcoin est une cryptomonnaie, oui, possible. On entend que Bitcoin est une blockchain, éventuellement, dépend de ce qu'on veut dire. On entend que Bitcoin est un protocole pair-à-pair (P2P), certainement. C'est un peu de tout ça. De la même façon que l'humain n'est ni une tête, ni deux bras, ni deux jambes.

Si Bitcoin est qualifié de (crypto)monnaie c'est parce qu'il le permet. Si Bitcoin est qualifié de Blockchain, c'est parce qu'il en a besoin et le produit. Si Bitcoin est qualifié de protocole c'est parce qu'il y en a un.

Finalement, c'est une chose aux apparence protéiformes si on zoom trop. Dire ce qu'est Bitcoin n'est foncièrement pas l'exercice le plus simple si la volonté est d'apporter une information complète (et ni le plus intéressant). C'est ce ça permet qui compte, et ce que ça permet : ce sont les contrats intelligents.

L'objet de cet article c'est de voir la tronche qu'ont ces fameux contrats, c'est juste en dessous !

> Vous aurez bien cerné qu'ici nous allons parler du corps des contrats, ce qu'ils sont matériellement. Ce qu'ils sont ontologiquement nécessite de parler d'autres parties qui composent Bitcoin, en l'occurence la preuve de travail, les transactions et leur timestamp, le protocole P2P, etc.

## Initiation aux contrats intelligents

Un contrat intelligent sur Bitcoin est composé de plusieurs parties.

__Prenons le temps de souligner que ce que nous allons voir concerne Bitcoin mais que le concept de ces objets est récurrent dans nombreuses autres technologies Blockchain comme Ethereum.__

### Au commencement était le verbe : Le SCRIPT et les OP CODE

Un contrat tacite est inscrit, et pour l'inscrire il faut une langue. Sur Bitcoin la langue utilisée est le `SCRIPT`. Les phrases en `SCRIPT` sont construites avec des `OP CODE`.

Le `SCRIPT` est donc un groupe d'opérations empilées et interprétées de haut en bas, c'est ce qui consistue _in fine_ le contrat intelligent.

Mettons il existe un `OP CODE` d'addition appelé `OP_ADD`, un `SCRIPT` permettant d'additionner `4 + 2` ressemblerait à ceci :

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

> Le concept du `OP CODE` (code opération) est apparu avec l'émergence des processeurs. Les processeurs prennent en entrée des valeurs binaires (des 1 ou des 0). Regroupées par 8 ces valeurs binaires forment des octets (ou bytes). Un `OP CODE` est un octet particulier quit peut être utilisé pour décrire une opération particulier : c'est l'`OP CODE`. En programmation l'`OP CODE` permet de définir universellement des opérations au sein d'un protocole car toutes les machines peuvent interpréter des octets.

C'est bien sympa tout ça, c'est plutôt archaïque vous ne trouvez pas ? Je vous rassure, c'est bien le cas. C'est une logique qui est appliquée en bas niveau, au plus proche des processeurs des machines. Au plus c'est bas niveau, au plus c'est logiquement simple... Mais ça devient vite illisible pour l'humain amoureux de la sémantique.

On a pas encore parlé de signature... Or le contrat concerne tout de même une ou plusieurs parties signataires ! Il est temps de voir le `scriptSig` et le `scriptPubKey`.

### Signé c'est pesé : le scriptSig et le scriptPubKey

D'un point de vue purement `SCRIPT` le `scriptSig` et le `scriptPubKey` forment à eux deux un `SCRIPT`... Rien de nouveau.

Cependant, ces deux élements amènent à s'intéresser à un concept du contrat intelligent qui dépasse le simple cadre du langage et son interprétation.

Pour revenir un peu sur le sujet d'un contrat, c'est un corpus qui implique un ou des parties. Un contrat est observé et vérifié par une tierce partie.

Dans le cadre d'un contrat intelligent, il peut être en suspend lorsqu'il n'est pas signé par un ou plusieurs parties : cela est régie par le script de verrouillage (ou _locking script_), c'est le `scriptPubKey`. Cette suspension est débloquée grâce au script de déverrouillage (ou _unlocking script_), c'est le `scriptSig`. Dans ce cadre, ce qui exécute le déverrouillage d'un contrat intelligent est le protocole Bitcoin (si le `scriptSig` est valide évidemment, c'est tout le jeu de l'observabilité et de la vérification). Le protocole Bitcoin a alors le rôle de tierce partie arbitrant le contrat qui implique un ou des parties.

OK super... Mais c'est quoi au fond le `scriptSig` et le `scriptPubKey` ?

Le `scriptSig` peut être essentiellement n'importe quoi servant de clé à l'exécution du contrat intelligent. C'est aucune, une ou plusieurs valeur(s), qui sont directement ajoutées sur le dessus de la pile `INPUT` du SCRIPT.

Le `scriptPubKey` est le SCRIPT qui servira à déterminer si le `scriptSig` est valide ou non.

> On notera deux choses intéressantes. La première c'est qu'un `scriptSig` vide et valide signifie que le contrat nécessite de rien pour être valide. La seconde c'est qu'un `scriptPubKey` peut très bien être un SCRIPT qui sera toujours invalide peu importe le `scriptSig`, ce qui correspond à un contrat corrompu (ce qui peut avoir une utilité).

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

Effectivement, tout peut être compris comme signature. Sauf que... Imaginez que je vous tende un contrat qui stipule "Je soussigné X ferai un virement de 1000EUR si l'autre partie signe la résolution x + y = 6". Allez-vous de prime abord l'accepter ce contrat ? Il semblerait que non, c'est aussi cela l'observabilité d'un contrat.

> Dans l'absurde exemple évoqué, vous remarquez la mention "l'autre partie". Une signature sert à authentifier une partie. Etant donné l'absurdité de la demande de signature, la résolution de x + y = 6, quiconque ayant une éducation mathématique ou un bon raisonnement logique peut être un signataire valide. Ce qui fait que le contrat est ouvert à toute partie. Et oui... Pas si intelligent le contrat, il ne peut pas savoir si le signataire est le signataire attendu dans notre exemple.

Hormis les cas bien particuliers, un contrat demande un `scriptSig` prouvant que la personne est l'unique personne étant capable de fournir la bonne valeur. Et pour cela on utilise la signature cryptographique. Nous allons le voir en exemple avec des cas réels ci-dessous.

## Première version de paiement (obsolète)

{{< video src="videos/btc_opcode_p2pk.mp4" >}}

```
scriptSig: <signature=304...0ef>
scriptPubKey: <pubKey=038...f7d> OP_CHECKSIG
```

## Paiement

{{< video src="videos/btc_opcode_p2pkh.mp4" >}}

```
scriptSig: <signature=304...667> <pubKey=03e...ba7>
scriptPubKey: OP_DUP OP_HASH160 <pubKeyHash=bb2...e3e> OP_EQUALVERIFY OP_CHECKSIG
```

## Gèle temporaire d'une transaction

{{< video src="videos/btc_opcode_freeze.mp4" >}}

```
scriptSig: <signature=304...488> <pubKey=023...da2>
scriptPubKey: <expiryTime=2000> OP_CHECKLOCKTIMEVERIFY OP_DROP OP_DUP OP_HASH160 <pubKeyHash=909...5e8> OP_EQUALVERIFY OP_CHECKSIG
```

## Puzzle Bitcoin

{{< video src="videos/btc_opcode_puzzle.mp4" >}}

```
scriptSig: <data=satoshi>
scriptPubKey: OP_HASH256 <hash=da2...26f> OP_EQUAL
```
+++
author = "Jonathan Serra"
title = "Bitcoin Script"
date = "2022-09-04"
description = "Vous avez un nœud Bitcoin et Lightning Network, vous participez à la décentralisation du réseau, bravo ! Cependant il réside un point noir, en particulier si votre noeud est localisé chez vous ou votre bureau... En connaissant votre adresse IP, publiée par le noeud, il est possible de vous localiser. On remédie à cela avec Tor."
tags = [
  "bitcoin", "debian", "bitcoin-cli", "tor"
]
image = "images/thumbnails/tor_bitcoind_lnd.png"
+++


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
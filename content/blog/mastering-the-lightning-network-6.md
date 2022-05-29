+++
author = "Jonathan Serra"
title = "6 - Mastering the Lightning Network - Comprendre la commitment transaction"
date = "2022-04-17"
description = "On lit le livre Mastering the Lightning Network (LN) en Direct ! Chapitre 6 - Architecture Lightning Network; Chapitre 7 - Les canaux de paiement"
tags = [
  "bitcoin", "lightning network", "mastering the lightning network", "live"
]
image = "images/thumbnails/mastering_the_lightning_network_6.jpg"
+++

## Lecture des chapitres 6 et 7 de Mastering the Lightning Network

{{< youtube id="8BheJvHN-gk" title="Comprendre la commitment transaction - Mastering the Lightning Network (Bitcoin)" >}}

Dans ce chapitre, nous allons plonger dans les canaux de paiement et voir comment ils sont construits. Nous commencerons par le nœud d'Alice ouvrant un canal vers le nœud de Bob, en nous appuyant sur les exemples présentés au début de ce livre.

Les messages échangés par les nœuds d'Alice et de Bob sont définis dans "BOLT #2 : Peer Protocol for Channel Management". Les transactions créées par les nœuds d'Alice et de Bob sont définies dans "BOLT #3 : Bitcoin Transaction and Script Formats". Dans ce chapitre, nous nous concentrons sur les parties "Channel open and close" et "Channel state machine" de l'architecture du protocole Lightning, mises en évidence par un aperçu au centre (couche peer-to-peer) des canaux de paiement dans la suite de protocoles Lightning .
+++
author = "Jonathan Serra"
title = "9 - Mastering the Lightning Network - Onion routing, Gossip protocol et Path Finding"
date = "2022-04-17"
description = "On lit le livre Mastering the Lightning Network (LN) en Direct ! Chapitre 10 - Routage en oignon; Chapitre 11 - Protocole de Gossip; Chapitre 12 - Path Finding"
tags = [
  "bitcoin", "lightning network", "mastering the lightning network", "live"
]
image = "images/thumbnails/mastering_the_lightning_network_9.jpg"
+++

## Lecture des chapitre 10, 11 et 12 de Mastering the Lightning Network

{{< youtube id="wIZqDOvhDJE" title="Onion routing, Gossip protocol et Path Finding - Mastering the Lightning Network (Bitcoin)" >}}

### Routage en oignon

Dans ce chapitre, nous décrirons le mécanisme de routage en oignon du Lightning Network. L'invention du routage en oignon précède le Lightning Network de 25 ans ! Le routage en oignon a été inventé par des chercheurs de la marine américaine en tant que protocole de sécurité des communications. Le routage en oignon est le plus célèbre utilisé par Tor, la superposition Internet routée en oignon qui permet aux chercheurs, aux militants, aux agents de renseignement et à tout le monde d'utiliser Internet de manière privée et anonyme.

Dans ce chapitre, nous nous intéressons à la partie "Source-based onion routing (SPHINX)" de l'architecture du protocole Lightning, mise en évidence par un aperçu au centre (couche de routage) du routage Onion dans la suite de protocoles Lightning.

### Protocole de Gossip

Dans ce chapitre, nous décrirons le protocole de potins du Lightning Network et comment il est utilisé par les nœuds pour construire et maintenir un graphe de canal. Nous passerons également en revue le mécanisme d'amorçage DNS utilisé pour trouver des pairs avec lesquels "commérer".

La section "Frais de routage et relais Gossip" est mise en évidence par un aperçu couvrant la couche de routage et la couche peer-to-peer du protocole Gossip dans la suite de protocoles Lightning.

### Path Finding

La livraison des paiements sur le Lightning Network dépend de la recherche d'un chemin de l'expéditeur au destinataire, un processus appelé recherche de chemin. Étant donné que le routage est effectué par l'expéditeur, l'expéditeur doit trouver un chemin approprié pour atteindre la destination. Ce chemin est ensuite encodé dans un oignon.

Dans ce chapitre, nous examinerons le problème de la recherche de chemin, comprendrons comment l'incertitude sur les équilibres des canaux complique ce problème et examinerons comment une implémentation typique de la recherche de chemin tente de le résoudre.
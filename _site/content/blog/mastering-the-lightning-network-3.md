+++
author = "Jonathan Serra"
title = "3 - Mastering the Lightning Network - Transaction d'engagement (commitment tx)"
date = "2022-04-17"
description = "On lit le livre Mastering the Lightning Network (LN) en Direct ! Chapitre 3 - Comment le Lightning Network fonctionne"
tags = [
  "bitcoin", "lightning network", "mastering the lightning network", "live"
]
image = "images/thumbnails/mastering_the_lightning_network_3.jpg"
+++

## Lecture du chapitre 3 de Mastering the Lightning Network

{{< youtube id="L0-U-hrjQdY" title="Transaction d'engagement (commitment tx) - Mastering the Lightning Network (Bitcoin)" >}}

Maintenant que nous avons suivi Alice alors qu'elle installait un portefeuille Lightning et achetait un café à Bob, nous allons regarder sous le capot et déballer les différents composants du Lightning Network impliqués dans ce processus. Ce chapitre donnera une vue d'ensemble de haut niveau et n'abordera pas tous les détails techniques. Le but est plutôt de vous aider à prendre conscience des concepts et briques de base les plus importants du Lightning Network.

Si vous avez de l'expérience en informatique, en cryptographie, en Bitcoin et en développement de protocoles, ce chapitre devrait vous suffire pour pouvoir remplir vous-même les détails de connexion. Si vous êtes moins expérimenté, ce chapitre vous donnera une vue d'ensemble suffisante pour que vous compreniez plus facilement les spécifications formelles du protocole, connues sous le nom de BOLT (Basis of Lightning Technology). Si vous êtes débutant, ce chapitre vous aidera à mieux comprendre les chapitres techniques du livre.

Nous commencerons par une définition en une phrase de ce qu'est le Lightning Network et nous le détaillerons dans le reste de ce chapitre.

Le Lightning Network est un réseau peer-to-peer de canaux de paiement mis en œuvre sous forme de contrats intelligents sur la blockchain Bitcoin ainsi qu'un protocole de communication qui définit la manière dont les participants configurent et exécutent ces contrats intelligents.
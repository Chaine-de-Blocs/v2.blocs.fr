+++
author = "Jonathan Serra"
title = "4 - Mastering the Lightning Network - Créer un réseau de nœud Lightning Network"
date = "2022-04-17"
description = "On lit le livre Mastering the Lightning Network (LN) en Direct ! Chapitre 4 - Les clients Lightning Network"
tags = [
  "bitcoin", "lightning network", "mastering the lightning network", "live"
]
image = "images/thumbnails/mastering_the_lightning_network_4.jpg"
+++

## Lecture du chapitre 4 de Mastering the Lightning Network

{{< youtube id="W2agBbOZIbI" title="Créer un réseau de nœud Lightning Network - Mastering the Lightning Network (Bitcoin)" >}}

Comme nous l'avons vu dans les chapitres précédents, un nœud Lightning est un système informatique qui participe au Lightning Network. Le Lightning Network n'est pas un produit ou une entreprise ; il s'agit d'un ensemble de normes ouvertes qui définissent une base d'interopérabilité. En tant que tel, le logiciel de nœud Lightning a été créé par diverses entreprises et groupes communautaires. La grande majorité des logiciels Lightning sont open source, ce qui signifie que le code source est ouvert et sous licence de manière à permettre la collaboration, le partage et la participation de la communauté au processus de développement. De même, les implémentations de nœuds Lightning que nous présenterons dans ce chapitre sont toutes open source et sont développées en collaboration.

Contrairement à Bitcoin, où la norme est définie par une implémentation de référence dans un logiciel (Bitcoin Core), dans Lightning, la norme est définie par une série de documents de normes appelés Basis of Lightning Technology (BOLT), trouvés dans le référentiel <a href="https://github.com/lightning/bolts/blob/master/00-introduction.md" target="_blank">lightning-rfc</a>.

Il n'y a pas d'implémentation de référence du Lightning Network, mais il existe plusieurs implémentations concurrentes, compatibles BOLT et interopérables développées par différentes équipes et organisations. Les équipes qui développent des logiciels pour le Lightning Network contribuent également au développement et à l'évolution des normes BOLT.

Une autre différence majeure entre le logiciel de nœud Lightning et le logiciel de nœud Bitcoin est que les nœuds Lightning n'ont pas besoin de fonctionner en parallèle avec des règles de consensus et peuvent avoir des fonctionnalités étendues au-delà de la ligne de base des BOLT. Par conséquent, différentes équipes peuvent poursuivre diverses fonctionnalités expérimentales qui, si elles réussissent et sont largement déployées, peuvent faire partie des BOLT plus tard.

Dans ce chapitre, vous apprendrez à configurer chacun des packages logiciels pour les implémentations de nœuds Lightning les plus courantes. Nous les avons présentés par ordre alphabétique pour souligner que nous ne préférons ou n'approuvons généralement pas l'un par rapport à l'autre. Chacun a ses forces et ses faiblesses, et le choix dépendra de divers facteurs. Puisqu'ils sont développés dans différents langages de programmation (par exemple, Go, C, etc.), votre choix peut également dépendre de votre niveau de familiarité et d'expertise avec un langage spécifique et un ensemble d'outils de développement.
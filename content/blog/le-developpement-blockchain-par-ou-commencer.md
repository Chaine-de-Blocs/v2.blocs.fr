+++
author = "Jonathan Serra"
title = "Le développement Blockchain, par où commencer ?"
date = "2021-05-17"
description = "La cryptomonnaie, la technologie Blockchain, ça fait beaucoup parler. Plus rarement pour l'innovation technique que la technologie apporte que pour son actualité sur les marchés. Les deux aspects ne sont pas inintéressants, d'ailleurs l'intérêt financier pour la technologie permet aussi de susciter un vif intérêt pour la faire évoluer. Et la technologie Blockchain évolue vite."
tags = [
  "bitcoin", "lightning network", "mastering the lightning network", "live"
]
image = "images/thumbnails/devblockchain.png"
+++

La cryptomonnaie, la technologie Blockchain, ça fait beaucoup parler. Plus rarement pour l'innovation technique que la technologie apporte que pour son actualité sur les marchés. Les deux aspects ne sont pas inintéressants, d'ailleurs l'intérêt financier pour la technologie permet aussi de susciter un vif intérêt pour la faire évoluer. Et la technologie Blockchain évolue vite.
C'est aujourd'hui une manne financière qui s'auto-finance. Avec toujours une demande d'innovation croissante allant de la DeFi (Decentralized Finance) aux NFTs (jetons non fongibles) qui ont maintenant le vent en poupe.
Vous avez un intérêt pour la technique, alors c'est le moment le plus opportun pour se lancer. Nous allons voir dans cet article tous les éléments importants pour s'initier au développement Blockchain. C'est parti ?
Afin de faciliter la lecture je vais utiliser le terme "Blockchain" pour évoquer toute l'innovation autour de la technologie Blockchain.

## L'intérêt de l'innovation Blockchain

Il y a encore quelques années, et ça semble être un poncif de plus en plus suranné, nous parlions souvent de "Blockchain Bullshit". Cette formulation peu racoleuse et à charge émergeait beaucoup avec l'arrivée de Ethereum dans l'écosystème en 2015.
Ethereum, l'enfant spirituel de Bitcoin et des projets connexes comme <a href="https://bitcoin.fr/colored-coins/" target="_blank">Colored Coins</a> (parmi les premiers jets du NFT).

Aujourd'hui, les galimatias des gardiens du temple sont moins audibles. De manière concrète la Blockchain propose des approches nouvelles voire diruptives si on s'intéresse  de près à la DeFi par exemple. Selon moi, la disruption était déjà là avec Bitcoin, j'aime parler d'une boîte de Pandore savamment ouverte.

La disruption, concept fort usité pour évoquer le néant parfois mais aussi pour soulever l'impact fondamental d'une innovation. Si la Blockchain a bien disrupté un domaine, on pourrait l'appeler "des contrats et de la confiance".

Vaste monde qui comporte une multitude de cases comme les interactions entre agents économiques, ou la juridiction et la loi. Concrètement, l'innovation Blockchain apporte un moyen de créer la confiance autour de contrats.

Cela semble bien peu ludique, ça manque de fun ! Mais rassurez-vous, on touche à des couches si profondes que les applications sont encore à découvrir ou à approfondir : allant de la création d'un circuit économique entre le monde réel et un jeux vidéo à la collection de cartes, à titre d'idées.

## Du Web à la Blockchain, le frontend

Entrons maintenant dans ce qui nous intéresse, la technique.

Et pour commencer, rien de mieux qu'introduire la partie la plus évidente et la plus accessible.
Lorsqu'on parle de développement Blockchain vous allez - quasiment - toujours retrouver une partie visible de l'iceberg : le frontend.

Par exemple il va s'agir de faire une interface pour un portefeuille, ou une page pour suivre des transactions. Ou encore mieux, une page pour réaliser des contrats !

Toutes ces couches sont largement développées avec les langages Web (JavaScript / TypeScript / Dart, HTML, CSS).
Même pour faire des applications de bureau ou mobile : <a href="https://www.electronjs.org/" target="_blank">Electron</a> pour l'application bureau, <a href="https://reactnative.dev/" target="_blank">React Native</a> ou <a href="https://flutter.dev/" target="_blank">Flutter</a> pour de l'application mobile.

### Web3JS, un premier pas

Web3JS, doux nom... Vous connaissez le Web 2.0, c'est celui que vous utilisez tous les jours, c'est le Web dynamique avec des sites comme Amazon ou Backmarket pour faire des achats. La Blockchain apporte le Web 3.0, bon c'est juste du babélisme technico-commercial. <a href="https://www.youtube.com/watch?v=rc744Z9IjhY" target="_blank">L'Internet de la valeur</a>,
ça a plus de caractère.

Je vous parle de <a href="https://web3js.readthedocs.io/en/v1.3.4/" target="_blank">Web3JS</a>, c'est une solution pour JavaScript (et adaptée à d'autres langages).
Cette dernière a l'intérêt d'être largement documentée et usitée.
Si vous ne savez pas par quoi commencer, Web3JS est votre <i>go to</i>.

Web3JS est labélisée comme la solution pour les APIs Ethereum. Dans les faits cette librairie peut aller bien au-delà de Ethereum. C'est ce qui va vous permettre de créer le lien entre ce qui se passe dans une blockchain et l'affichage sur un site ou une application.

Voilà à quoi ressemble du code avec Web3JS :

{{< highlight javascript >}}
// Connecte Web3 avec un provider par défaut.
// Un provider est un logiciel qui permet de faire l'interfaçage entre le Web3 et
// le réseau Blockchain (mainnet, testnet, autre...).
// En général le "givenProvider" sera Metamask. Metamask est une extension installée
// sur le navigateur de l'utilisateur qui joue le rôle à la fois de portefeuille
// et de gestionnaire de providers
let web3 = new Web3(Web3.givenProvider);

// Exemple de création et d'envoi d'une transaction
web3.eth.sendTransaction({
    from: '0xDD135B5D7AC09183C3A975e989a57D65f6c8B54B', // Adresse vers laquelle envoyer des jetons
    value: Web3.utils.toWei(1), // Transforme la valeur 1 en unité Wei
  })
  .once('sending', function(payload){
    /* La transaction est en cours d'envoi */
  })
  .once('sent', function(payload){
    /* La transaction a été envoyée au provider */
  })
  .once('transactionHash', function(hash){
    /* Le provider a reçu la transaction et a renvoyé son identifiant */
  })
  .once('receipt', function(receipt){
    /* Reçu de la transaction, cela signifie qu'elle est considérée par le réseau en but de la publier dans la blockchain */ 
  })
  .on('confirmation', function(confNumber, receipt, latestBlockHash){
    /**
    La transaction est maintenant inscrite dans la blockchain.
    La confirmation est déclenchée pour chaque nouveau bloc publié après le bloc
    comportant cette transaction
    **/
  })
  .on('error', function(error){
    /* Oops, y'a eu une erreur */
  })
  .then(function(receipt){
    /* La transaction est maintenant inscrite dans la blockchain */
  });
{{< /highlight >}}

> Binance Smart Chain (BSC) est un réseau lancé par la fameuse
plateforme Binance. Ce dernier étant repris de Ethereum peut fonctionner avec Web3JS.

> Web3 de manière générale est une librairie qui permet de faire le dialogue entre une blockchain
et une application, qu'elle soit un client ou un serveur.

### Par où commencer ?

Si vous n'avez aucune notion du développement Web, alors commencez par là ! Vouloir directement faire du développement Blockchain en frontend va créer une confusion qui vous ralentira.

Une excellente ressource française sont les cours gratuits de <a href="https://openclassrooms.com/fr/courses/6175841-apprenez-a-programmer-avec-javascript" target="_blank">OpenClassroom</a>.

Pour s'initier concrètement lancez-vous sur un projet faisant usage de la Blockchain, par exemple une page qui permet de faire des transactions. Pour faire cela vous n'aurez pas besoin de faire autre chose que du développement frontend. <a href="https://www.npmjs.com/package/webjs" target="_blank">Web3JS</a> et <a href="https://metamask.io/" target="_blank">Metamask</a> seront vos outils principaux.

## L'arrière scène de la Blockchain, le backend

Initialement, l'innovation Blockchain arrivée avec Bitcoin est un projet majoritairement backend. Le gros du travail sur Bitcoin est son protocole pair-à-pair. La petite partie frontend est le portefeuille <a href="https://bitcoin.org/fr/telecharger" target="_blank">Bitcoin Core</a> développé en C++ avec Qt (Bitcoin Qt était son nom encore en 2013).

Ethereum est aussi un projet où la composante dominante est le protocole et sa machine virtuelle (EVM). Ethereum a cependant apporté un nouveau pan qui <a href="https://www.youtube.com/watch?v=lVGflsXnKh0" target="_blank">peinait à émerger avec Bitcoin pour des raisons techniques</a>.

La partie backend dans le développement Blockchain touche à plusieurs couches fondamentalement différentes.

### Choix 1 : Le développement de protocole

On va s'astreindre de reprendre la définition même de "protocole". C'est plus intéressant de se borner à ce qu'est un protocole autour de la technologie Blockchain.

Développer un protocole revient à développer sa Blockchain. C'est l'acte de définir des règles de comportements dans un réseau décentralisé. C'est la partie la moins évidente, pour cause  faire un protocole demande une ingénierie poussée. C'est un long travail de conception.

C'est un domaine d'expertise qui a relativement peu de demande, développer son propre protocole demande un investissement conséquent pour un gain modéré. D'autres approches sont souvent plus abordables. Elles sont évoquées plus bas.

#### Par où commencer ?

Pour s'initier à cette partie il est primordial d'avoir une bonne expérience sur un langage compilé comme Rust, C++ ou Golang, pas de chapelle ce n'est qu'une liste non exhaustive.
Il est possible de faire un protocole Blockchain avec un langage interprété comme JavaScript et le <i>soft</i> NodeJS. <a href="https://github.com/LiskHQ/lisk-core" target="_blank">Lisk</a> l'ont fait par exemple.

Vous pouvez aussi reprendre la source d'un protocole déjà développé, changer quelques règles dans le code source, et le publier. On parle à ce moment de <i>fork</i>. C'est ce que BCH ont fait avec Bitcoin Core, ou Dogecoin avec Litecoin qui lui est un fork de Bitcoin Core, ou Binance Smart Chain avec Ethereum.

### Choix 2 : Le développement de contrats intelligents

Là on commence à entrer dans ce qui est fortement demandé dans le marché des compétences en développement Blockchain.

Je vous évoquais plus tôt que là où la technologie Blockchain est disruptive, c'est autour des contrats et de la confiance entre agents économiques. Eh bien... L'innovation Blockchain va avec les <b>contrats intelligents</b>.

Simplement dit, le contrat intelligent est un programme dont l'exécution est connue et autonome. Elle est Connue car c'est un programme dont le code source est accessible à quiconque.

Imaginez deux acteurs dans une opération immobilière. Le cessionnaire Tom, et l'acquéreuse Adèle. Les deux associent leur échange à un contrat intelligent. À la signature par les deux acteurs, le contrat exécute une passation de la propriété à Adèle et un envoi des fonds à Tom est automatiquement exécuté. Sans ces deux signatures, rien ne se passe. On pourrait étendre avec
une histoire de délai avant mise à terme, etc. Vous avez saisi l'idée.

Les possibilités d'un contrat intelligent sont indénombrables puisque c'est un programme que vous développez. En général, n'importe quel secteur impliquant plusieurs acteurs qui ont besoin de confiance pourraient trouver un intérêt dans le contrat intelligent.

Côté développement il existe tout un univers mais une solution se dégage largement aujourd'hui. C'est Ethereum avec son langage <a href="https://bitconseil.fr/solidity-langage-ethereum/" target="_blank">Solidity</a>.

Solidity est un langage qui permet simplement de développer des contrats intelligents. C'est un langage typé qui reprend des principes de la programmation orientée objet comme Java.

#### Par où commencer ?

Vous pouvez dès maintenant vous amuser à faire des contrats intelligent avec l'éditeur Web <a href="https://remix.ethereum.org/" target="_blank">Remix</a>. Cela peut être déroutant au premier abord, en s'appuyant avec la documentation Remix et Solidity vous pouvez commencer à découvrir.

Un éminent livre complet a été écrit par l'inénarrable Andreas M. Antonopoulos avec Gavin Wood : <a href="https://github.com/ethereumbook/ethereumbook">Mastering Ethereum</a>.
Ce livre est en prix libre mais sa version française n'est pas trouvable sur le Web. C'est un contenu parfait pour cerner les tenants et les aboutissants d'Ethereum, néanmoins il manque en pragmatisme pour gagner en compétences rapidement.

### Choix 3 : Le développement de services connectés à un réseau Blockchain

C'est l'un des moins évoqués pourtant la demande pour ce genre de compétences est aussi importante que le développement de contrats. Ce genre de développement est plus proche des solutions classiques comme faire une API pour un service.

Dans un réseau Blockchain beaucoup d'informations circulent, elles sont toutes publiques par définition, si on se tient à une Blockchain publique.

Des services peuvent avoir un besoin d'exploiter ces informations dans leur infrastructure. Par exemple, le cas typique sont les places de marché dans la cryptomonnaie. Ces dernières gèrent des flux de transactions sur différentes cryptomonnaies. Pour chaque, le serveur de ces places de marché ont un service qui consulte les informations sur les différentes
blockchains.

Cette partie est souvent associée au développement de contrats intelligents. Dans la grande majorité des cas, après le développement d'un contrat intelligent, on souhaite tracer dans un serveur certaines informations pour avoir un suivi. Par exemple, il est possible de déclencher une action en temps réel lorsqu'une transaction est exécutée dans le réseau Blockchain avec WebSocket.

Je vous évoquais que Web3JS possède des librairies aussi pour d'autres langages dans le but lire des informations sur Ethereum.

Je vous montre un exemple de code commenté sur Golang avec `go-web3` :

{{< highlight go >}}
package main

import (
    "fmt"
    "github.com/umbracle/go-web3/wallet"
    web3 "github.com/umbracle/go-web3"
    "github.com/umbracle/go-web3/jsonrpc"
    "math/big"
)

func main() {
    // Connexion à un réseau blockchain local
    // Ici localhost:7545 est un provider JSONRPC
    client, err := jsonrpc.NewClient("http://localhost:7545")
    if err != nil {
      panic(err)
    }

    // On génère un nouveau portefeuille, la clé est une clé privée
    key, _ := wallet.GenerateKey()

    to := web3.Address{0x1}
    transferVal := web3.Ether(1)

    // Envoi de 1 Ether vers l'adresse 0x1
    txn := &web3.Transaction{
      To:    &to,
      Value: transferVal,
      Gas:   100000,
    }

    // Signature de la transaction
    signer := wallet.NewEIP155Signer(chainID)
    txn, _ = signer.SignTx(txn, key)

    // Envoi de la transaction
    data := txn.MarshalRLP()
    hash, _ := c.Eth().SendRawTransaction(data)
}
{{< /highlight >}}

#### Par où commencer ?

Le plus simple est de lancer votre projet et exploiter Ethereum qui a un nombre conséquent de librairies. Web3 est sûrement disponible pour votre langage favoris.

Commencez par lancer votre Blockchain Ethereum locale avec <a href="https://geth.ethereum.org/downloads/" target="_blank">Geth</a> ou <a href="https://www.trufflesuite.com/ganache" target="_blank">Ganache</a>. Avec cette dernière, vous serez capable de faire une API qui récupère les informations.

Par exemple, vous pouvez faire un service qui se connecte à votre Blockchain locale puis qui réagit à chaque transaction via le WebSocket.

## Conclusion

Vous aurez compris, j'axe l'article vers le plus pratique et le plus probant dans la quête de travailler dans ce domaine et créer de la valeur.

En plus de développer des compétences pour réaliser vos projets Blockchain, un tel bagage ouvre des portes à des métiers divers. Cela peut passer par de l'encadrement de projets pour des entreprises nécessitant d'une expertise à de l'audit de projets Blockchain.

On pourrait faire le tour de l'écosystème, évoquer plus en profondeur les projets tout autant intéressants comme Bitcoin, Cardano ou Algorand. Le bestiaire est incommensurable et c'est preuve de bonne santé, la vie grouille.

Ethereum est donc aujourd'hui l'écosystème le plus complet avec un historique fort. On compte parmi les grands tournants : les ICO avec ERC20, CryptoKitties et les NFT, ou encore la DeFi avec Uniswap.


## The Address Resolution Protocol

* La logique du routage IP requière que les hosts et routers encapsulent les packets IP à l'intérieur de trames de couche data-link. Pour les interfaces Ethernet, comment un routeur connait quelle addresse MAC utiliser pour la destination ? Il utilise ARP.

* Dans les LANs Ethernet, à chaque fois q'un host ou un router à besoin d'encapsuler un Packet IP dans une nouvelle trame Ethernet, l'host ou le router connaît soit l'addresse IP du prochain device, soit l'addresse IP d'un autre host ou l'addresse IP par défaut du router. Un router connaît la route IP utilisée pour transférer le packet IP, qui liste l'addresse IP du prochain router. Néanmoins, les hosts et routers ne connaissent pas au préalable les addresses MAC des périphériques voisins. 

TCP/IP définit l'ARP comme la méthode par laquelle n'importe quel host ou router dans un LAN peut dynamiquement découvrir l'addresse MAC d'un autre IP host ou router dans le même LAN. ARP définit un protocol qui inclut l'**ARP Request**, un message qui fait la simple requête "Si c'est ton addresse IP, stp répond avec ton addresse MAC". ARP définit aussi le message **ARP Reply**, qui énumère à la fois l'addresse IP originale et l'addresse MAC correspondante. 

La figure suivante montre l' ARP Request envoyée par le router R3 (à gauche, comme un broadcast LAN). Tous les devices sur le LAN vont ensuite traiter la trame reçue. A droite, à l'étape 2, l'host PC2 renvoie une ARP Reply, identifiant l'addresse MAC de PC2. 
Le texte à côté de chaque messages montre le contenu à l'interieur du message ARP, qui permet à PC2 de découvrir l'addresse IP de R3 et sa MAC correspondante, de même R3 apprend l'addresse IP et MAC correspondante de PC2.

<img src="m/1.png"> 


Les hosts et routers se rappellent des résultats ARP, en gardant les informations dans leur cache ou table ARP. Un host ou router n'ont besoin d'utiliser ARP qu'occasionnellement, pour construire le cache ARP la première fois. A chaque fois d'un host ou router à besoin d'envoyer un packet encapsulé dans une trame Ethernet, il va en premier rechercher dans son cache ARP l'addresse IP correcte et sa MAC correspondante. Les hosts et routers vont laisser les entrées de cache ARP sécouler pour nettoyer la table, donc des requêtes ARP occasionnelle peuvent être vue. 

```
arp -a
```

Autre définition : 

* Deux types d'addressage sont utilisé pour identifié les network hosts - l'addresse IP (couche 3) et l'addresse locale (couche Data Link). La couche Data Link correspond aussi grossièrement à l'addresse MAC. La résolution d'addresse (ARP), comme définie dans RFC 826, est le process dans lequel l'IOS determine l'addresse de couche Data Link (MAC) depuis l'addresse IP (couche Reseaux). 

ARP traduit une IP connue en MAC. Quand un host à besoin de transférer de la donnée à travers un réseaux, il a besoin dde savoir l'addresse MAC de destination. L'host va regarder son cache ARP et si la MAC n'est pas là, il va envoyer un message de Broadcast ARP pour trouvé l'host de destination, comme illustré : 

<img src="m/2.png"> 


## Variable-Length Subnet Masks 

* VLSM veut simplement dire que la conception du subnet utilise plus d'un mask dans la même classe de réseaux. VLSM à plusieurs avantages et désavantages, mais le challenge principal est qu'une conception de sous-réseaux qui utilise VLSM requière plus de math, et necessite aussi de penser à certains autres problèmes 

### VLSM : Concepts et Configurations 

* VLSM se produit lorsqu’un interréseau utilise plus d’un masque pour différents sous-réseaux d’un réseau unique de classe A, B ou C. La figure suivante montre un exemple de VLSM utilisé dans le réseau 10.0.0.0 de classe A .

VLSM in Network 10.0.0.0: Masks /24 and /30 : 
<img src="m/2.png"> 

* La figure montre un choix typique d'utilisation d'un masque /30 sur les liaisons série point à point, avec le masque /24 pour les LAN de sous-réseaux. Tous les sous-réseaux font partis du réseaux de classe A 10.0.0.0, avec deux masques utilisés, et ainsi la configuration de ce réseaux rentre dans la définition de VLSM. 

* Bizarrement, une erreur courrante est faite lorsque les gens pensent que VLSM signifie "Utilisation de plus d'un mask dans un inter-réseaux" plutôt que "Utilisation de plus d'un mask dans *un seul réseaux de classe*" . Par exemple, si dans un diagramme d’interréseau, tous les sous-réseaux du réseau 10.0.0.0 utilisent un masque 255.255.240.0, et tous les sous-réseaux du réseau 11.0.0.0 utilisent un masque 255.255.255.0, la conception utilise deux masques différents. Cependant, le réseau de classe A 10.0.0.0 utilise uniquement un masque, et le réseau de classe A 11.0.0.0 utilise un seul masque. Dans ce cas, la conception n'utilise pas VLSM.

* VLSM offre de nombreux avantages pour les réseaux réels, principalement liés à la façon dont vous allouez et utilisez votre espace d’adresse IP. Parce qu’un masque définit la taille du sous-réseau (le nombre des adresses hôtes dans le sous-réseau), VLSM permet aux ingénieurs de mieux répondre aux besoins adresses avec la taille du sous-réseau. Par exemple, pour les sous-réseaux qui ont besoin de moins d’adresses, le technicien utilise un masque avec moins de bits hôtes, donc le sous-réseau a moins d’adresses IP hôtes. Cette flexibilité réduit le nombre d’adresses IP gaspillées dans chaque sous-réseau. En gaspillant moins adresses, il reste plus d’espace pour allouer plus de sous-réseaux.


VLSM can be helpful for both public and private IP addresses, but the benefits are more
dramatic with public networks. With public networks, the address savings help engineers
avoid having to obtain another registered IP network number from regional IP address
assignment authorities. With private networks, as defined in RFC 1918, running out of
addresses is not as big a negative, because you can always grab another private network
from RFC 1918 if you run out.

# La carte APC

Tout d'abord, vous devriez savoir de quoi nous parlons, alors jetez un œil aux [schémas de l'APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/APC_schematics.pdf). J'ai essayé de concevoir la carte de manière simple, ce qui signifie qu'avec un peu de connaissances électronique, cela ne devrait pas être un problème pour vous de comprendre comment elle fonctionne.

Sur le schéma le nom des connecteur est donnée pour les System 7 et 11, ceux des system 7 sont aussi valides pour les system 3-6 et ceux des system 11 pour les system 9. L'image suivante vous montre l'emplacement des différents connecteurs. Le sens d'implentation est imprimé sur la carte mais est aussi visible sur l'image: le coté ayant le clip est représenté par une ligne supplémentaire sur le dessin.
Le connecteur GND pour les system 11 (1J3) a seulement 4 pins, sur l'image il est représenté par un rectangle noir. Le symbole X indique le pin a supprimer.

![Connecteurs APC](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/APC_Connectors.png)

L'APC n'est pas vraiment destiné aux débutants, sans connaissances en électronique vous risquez d'endomage votre flipper si vous ne faites pas les choses correctement.

## Achter une carte

Je recommande d'utiliser [JLCPCB](https://jlcpcb.com) comme fabriquant, car els fichiers d'assemblages sont compatible avec leurs spécifications. Vous devez commande au moins 5 cartes, alors vous pouvez demander sur les forums si des persones peuvent être interessé afin de faire profiter la production.
Si vous souhaitez en commander, vous devrez fournir le fichier APC_Gerber.zip pour les faire fabriquer. Vous devez aussi sélectionne 'PCB Assembly' et choisir 'top side' pour l'assemblage des composants. Dans l'étape suivant vous devrez envoyer le fichier 'APC_BOM.csv' et 'APC_cpl_top.csv' pour leur permettre de peupler la carte. Tous les fichiers nécessaire sont disponible [ici](https://github.com/AmokSolderer/APC/tree/master/DOC/Hardware/APC_FabricationFiles_SSOP).

Cependant, il est devenu de plus en plus difficile d'obtenir tous les composants nécessaires. Dans ce cas, JLCPCB indiquera un "Inventory shortage" dans la liste des pièces. Vous pouvez soit essayer de sélectionner une autre pièce avec le même boîtier, soit précommander la pièce. Rendez-vous dans le "Parts Manager" pour cette solution. Si la précommande a réussi, vous pouvez utiliser ces pièces pour vos cartes.

Notez que JLCPCB assemblera les connecteurs Molex, mais ils ne retireront pas les pin key. Cela peut être fait facilement en les chauffant avec un fer à souder et en les retirant avec une pince.

## Les composants

La plupart des composants peuvent être montés par le fabricant de la carte. Ils peuvent effectuer toutes les soudures en surface (SMD) et la plupart des autres opérations également. Par conséquent, il ne reste que quelques composants à monter. Vous pouvez trouver une liste de ces composants avec leur numéro de commande respective chez Mouser et Reichelt dans le fichier [Bill of Materials](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/Assembly/APC_BOMselfSolder.pdf).

Le détaillant allemand d'électronique Reichelt ne vend pas tous les composants nécessaires, mais certains (en particulier les connecteurs) sont bien moins chers par rapport à Mouser. Pour les Européens, il peut être judicieux de les commander séparément.

### disponibilité du TDA7496

J'ai reçu une notification de Mouser indiquant que l'amplificateur audio TDA7496 ne sera plus produit.

Pour le moment, je n'ai pas le temps de sélectionner un nouvel amplificateur, de modifier la conception de la carte et d'effectuer les tests nécessaires. Je pourrais le faire à l'avenir si le circuit intégré n'est plus disponible.
Cela signifie que vous devriez essayer d'obtenir ces circuits intégrés avant de commander les cartes.

## L'assemblage

Comme les noms des composants sont imprimés sur les cartes aux emplacements correspondants, vous pouvez simplement utiliser le [Bill of Materials](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/Assembly/APC_BOMselfSolder.pdf) pour identifier le bon composant à placer à cet endroit.
Le réseau de résistances RR8 doit être installé dans la bonne orientation. Il y a une marque pour la broche 1 imprimée sur les cartes APC. Sur les réseaux de résistances, la broche 1 est généralement marquée d'un point.

## Préparer la carte avant l'installation

Branchez l'Arduino DUE sur votre carte APC, mais ne montez pas encore le Raspberry Pi. Je recommande de faire les tests de base avant d'installer le Pi.
La prochaine étape consiste à [Téléverser le logiciel](https://github.com/AmokSolderer/APC/blob/master/DOC/Upload_SW.md). Je ferais cela avant de placer la carte APC dans votre flipper, car si cela fonctionne, vous saurez que votre alimentation de 5V n'a pas de court-circuit et fonctionne correctement.
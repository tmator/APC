# Comment se lancer

Les étapes suivantes sont nécessaires pour que l'APC contrôle votre flipper Williams System 3 - System11 :

1. La carte APC, comment la fabriquer
2. Installer le logiciel
3. Préparer votre machine
4. Configurer le BaseCode
5. Faire les tests
6. Lancer votre jeu

## La carte APC

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

## The Components

As most of the components are SMD and have already been populated by the board manufacturer there's only a few components left for you to assemble. You can find a list of these components with their respective order number from [Mouser](http://www.mouser.com) and [Reichelt](http://www.reichelt.de) in the [Bill of Materials](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/Assembly/APC_BOMnonSMD.pdf)

The german electronics retailer Reichelt doesn't sell all required components, but some (especially connectors) are much cheaper compared to Mouser, so for people in Europe it might still make sense to order them separately.

### Les composants

La plupart des composants peuvent être montés par le fabricant de la carte. Ils peuvent effectuer toutes les soudures en surface (SMD) et la plupart des autres opérations également. Par conséquent, il ne reste que quelques composants à monter. Vous pouvez trouver une liste de ces composants avec leur numéro de commande respective chez Mouser et Reichelt dans le fichier [Bill of Materials](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/Assembly/APC_BOMselfSolder.pdf).

Le détaillant allemand d'électronique Reichelt ne vend pas tous les composants nécessaires, mais certains (en particulier les connecteurs) sont bien moins chers par rapport à Mouser. Pour les Européens, il peut être judicieux de les commander séparément.

## L'assemblage

Comme les noms des composants sont imprimés sur les cartes aux emplacements correspondants, vous pouvez simplement utiliser le [Bill of Materials](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/Assembly/APC_BOMselfSolder.pdf) pour identifier le bon composant à placer à cet endroit.
Le réseau de résistances RR8 doit être installé dans la bonne orientation. Il y a une marque pour la broche 1 imprimée sur les cartes APC. Sur les réseaux de résistances, la broche 1 est généralement marquée d'un point.

## Preparation

### SD card adapter

Pour utiliser des cartes SD avec l'APC, un adaptateur est nécessaire. Il peut être construit très facilement en utilisant simplement un adaptateur micro SD et en soudant une rangée de broches en dessous.

![SD adapter](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/SDadapter.JPG)

Cet adaptateur doit ensuite être inséré dans le connecteur de carte SD P8 de l'APC. L'orientation doit être respectée. Regardez les photos de l'APC à l'intérieur de mes machines si vous n'êtes pas sûr de l'orientation. Dans la version 0.31 un adaptateur de carte MicroSD à été ajouté sous l'arduino, cet adaptateur n'est plus necessaire.

### Les rallonges de câbles pour System11b + c et DE

Pour certains jeux System11, vous devrez peut-être prolonger certains câbles pour les adapter à l'APC. Pour les cartes System11a, cela n'est pas nécessaire, mais vous devrez couper quelques colliers pour ouvrir un peu le faisceau de câbles. Si vous ne souhaitez pas faire cela, vous pouvez utiliser des extensions de câbles à la place.

Pour les jeux ultérieurs qui comportent des cartes d'alimentation auxiliaire et de connexion, il n'y a aucun moyen d'éviter cela. Pour une machine System11c comme Rollergames, vous devrez prolonger les câbles jusqu'au connecteur d'alimentation 1J17, le connecteur de strobe de lampe 1J7 et l'un des drivers de solénoïde 1J19. Pour les drivers de flipper (deuxième partie de 1J19), il suffit de couper un collier de serrage. Le résultat ressemble à ceci :

![APC_Rollergames](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/APC_Rollergames.JPG)

### Les cables d'afficheur System7

Les jeux Sys3 - 7 ont besoin d'un seul fil pour le blanking. Sur l'APC, vous pouvez utiliser soit la broche 12 des segments d'affichage 1 (1J5), soit P3 qui est une seule broche située juste à côté de 1J5 et étiquetée Blank_N.   Ce signal doit être connecté à la broche 4 de 1P3 (le connecteur qui était branché sur 1J3 de votre ancienne carte CPU). Comme c'est la seule broche fonctionnelle sur 1P3, une manière simple de faire la connexion est de simplement tourner 1P3 de 90 degrés et de le brancher sur la broche 12 de 1J5 sur l'APC. L'image ci-dessous montre cette configuration dans un Jungle Lord System7.

![Sys7DispCable](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Sys7DispCable.JPG)

La photo montre également les deux connexions supplémentaires à effectuer pour que les virgules des affichages Sys7 fonctionnent. L'APC les a sur la broche 10 (virgules 1+2) et 11 (virgules 3+4) de 1J5 et sur la broche 3 (virgules 1+2) et la broche 4 (virgules 3+4) de 1J8. L'une ou l'autre de ces broches doit être connectée à la broche 2 (virgules 1+2) et à la broche 1 (virgules 3+4) de l'ancien 1P8.
Sur la photo, 1J8 de l'APC a été utilisé pour fournir les signaux.

### Les cable audio pour les system 7

Vous pouvez utiliser un simple câble à 5 fils pour connecter les cartes audio System7 avec l'APC 3.0 (les anciens APC nécessitent du matériel supplémentaire). Ce câble doit connecter les broches 3 à 7 de l'interface d'extension HW (P11) de l'APC 3.0 à la carte audio. Une manière simple de faire cela est de les connecter aux broches 12 à 8 de 1P8 (la fiche appartenant à 1J8 de votre carte CPU Sys7).

Un câble de ce type est montré ci-dessous.

![Sys7SoundCable](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Sys7SoundCable.jpg)

Pour que l'APC utilise cette interface, vous devez définir le paramètre 'PinMame Sound' sur 'Board'. Vous pouvez trouver ce paramètre dans le menu 'Game Settings' en mode 'Remote Control'.

## Préparation du logiciel

Pour compiler le logiciel, vous avez besoin au minimum de l'IDE Arduino avec les bibliothèques SPI et SdFat installées.
Dans l'IDE, vous devez sélectionner "Arduino DUE (Programming Port)" comme carte cible. Assurez-vous d'avoir tous les fichiers .ino et le fichier Sound.h du dépôt APC dans votre répertoire de travail. Le nom de ce répertoire doit être "APC".

## Le son

Pour que l'APC gère les sons et de la musique, vous devez d'abord préparer les fichiers avec l'outil AudioSave, puis les mettre sur la carte SD (consultez la page [outils logiciels utiles](https://github.com/AmokSolderer/APC/blob/master/DOC/UsefulSWtools.md) pour plus de détails).
Nous sommes toujours à la recherche de quelqu'un pour mettre les fichiers audio sur un serveur web, mais pour l'instant, vous devez me contacter pour obtenir les fichiers audio nécessaires pour exécuter les jeux. Une liste des ensembles de fichiers sonores disponibles peut être trouvée sur la [section PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md).
Cela vaut également pour mon jeu Black Knight, qui utilise également les fichiers sonores PinMame pour Black Knight, plus quelques son supplémentaires.

L'APC dispose d'un contrôle de volume numérique qui vous permet d'ajuster le volume dans les paramètres du système. Mais avant de régler le volume sur une valeur différente de zéro, assurez-vous que le potentiomètre de volume n'est pas connecté (10J4 pour Sys3-7 et 1J16 pour Sys9-11), sinon les niveaux de volume s'additionneront et vous pourriez endommager vos haut-parleurs. Je vous recommande de ne pas du tout utiliser 10J4, mais seulement le contrôle de volume numérique.

## Préparer la carte avant l'installation

Branchez l'Arduino DUE sur votre carte APC, mais ne montez pas encore le Raspberry Pi. Je recommande de faire les tests de base avant d'installer le Pi.
La prochaine étape consiste à [Téléverser le logiciel](https://github.com/AmokSolderer/APC/blob/master/DOC/Upload_SW.md). Je ferais cela avant de placer la carte APC dans votre flipper, car si cela fonctionne, vous saurez que votre alimentation de 5V n'a pas de court-circuit et fonctionne correctement.

Vous êtes maintenant prêt a lancer les [tests initiaux](https://github.com/AmokSolderer/APC/blob/master/DOC/InitialTests.md).

## Paramètres

Une liste des paramètres et une brève description de leur utilisation peuvent être trouvées sur la [page des paramètres](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md).

## Faire son propre logiciel

Un fois que votre carte est prête, vous avez les options suivantes :

* Ecrire son propre jeu en C. Pour cela jetez un oeil à[la documentation d'APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/APC_SW_reference.pdf) pour connaitre les différentes fonctions de l'API et utiliser le  [Tutoriel](https://github.com/AmokSolderer/APC/blob/master/DOC/GameCodeTutorial.md) pour se familiariser avec.

* Créer un jeu avec MPF; il y a pas mal de doc sur le [site offciel](http://missionpinball.org/).

* Utiliserl'émulateur PinMame pour exécuter le code original. Vous trouverez plus d'informatios dans la [section PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md).
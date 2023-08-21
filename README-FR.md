
# L'Arduino Pinball Controller

Avant d'expliquer ce qu'est APC, clarifions ce que ce n'est pas.

APC n'est pas une carte qui permet de remplacer simplement une CPU. Elle est destinée aux personnes ayant des connaissances en électronique et programmation qui souhaitent étendre les capacités de leurs flippers. 

Ce projet est sans but commercial , vous pouvez utiliser APC à vos risques et périls et je ne peux être tenu responsable de quelconques dommages sur votre machine.
Certains flipper ne fonctionnent pas avec la carte APC ou certains ont besoin de matériel supplémentaire. Allez voir la section [Problèmes connus](https://github.com/AmokSolderer/APC/blob/master/README-FR.md#known-issues) pour plus de détails.

## Présentation


L'APC est une carte programmable, pour les flippers Williams. Elle utilise un Arduino DUE et contient tout les composants nécessaire pour faire fonctionner les flipper Williams System 3 à 11c (et les Data East Alphanumériques) :
* Supporte tous les types d'affichages
* 24drivers  solénoïdes
* Matrice de 64 lampes
* Matrice de 64 contacts + 8 contacts autre
* Carte SD (pour stocker les fichiers audio, les paramètres et les scores)
* Amplificateur audio 2 cannaux (pour jouer simultanément la musique et les effets sonores / ou jouer en stéréo)

Pour résumer, elle remplace CPU, carte de puissance et carte son pour un coût avoisinant les 100 euros.

![APC 2.0](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/APC.JPG)

L'image ci-dessus présente APC 3.1 avec le nouveau lecteur de carte SD installé sous l'Arduino.

Regardez ma  [vidéo APC 3](https://www.youtube.com/watch?v=4EgOTJyxMXo) pour découvrir ce que propose cette carte.

Il y a un sommaire complet de la documentation disponible au bas de cette page.

### Fonctionnalités spéciales

* Interface d'extension. Le brochage est compatible avec la carte (C-13287) Sound Overlay Solenoid utilisée dans le Whirlwind, mais elle peut être utilisé avec d'autres types extensions. 
* Un port permet de connecter un Raspberry afin de jouer à la rom originale via PinMame.
* [Les exceptions PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameExceptions.md) permettent d'ajouter des actions particulière en fonction des évènement dans le jeu. Cela permet de modifier le déroulé du jeu original exécuté par PinMame. Regardez ma vidéo sur  [Jungle Lord](https://www.youtube.com/watch?v=bbfhH_-gMfE) ou  [Comet ball saver](https://youtu.be/JbgMa_pn0Lo) pour découvrir comment quelques lignes de code peuvent changer les règles.

Les exemples d'utilisation des exceptions PinMame sont : 

* Pour les jeux avant system 11 qui on juste un canal sonore, vous pouvez ajouter une musique de fond.  C'est quand même plus fun de jouer au Disco Fever avec en fond de la musique Disco non ?
* Ajouter des "toys" comme un shaker ou des flasheurs et les règles nécessaires pour les commander. Si votre machine n'a pas de driver solénoid disponible, vous pouvez en ajouter avec [la carte d'extension solénoide](https://github.com/AmokSolderer/APC/blob/master/DOC/SolExpBoard.md) pour en piloter 8 de plus.Vous pouvez aussi tuiliser la  [carte d'extension de LED ](https://github.com/AmokSolderer/APC/blob/master/DOC/LEDexpBoard.md) pour piloter des bandeaux de LEDs RGB.
* Ajouter un "ball saver". C'est une action assez facile à ajouter. Tout ce que vous avez à faire c'est de ne pas dire a PinMame que la bille est arriver dans le "outhole", mais de l'éjecter dans la voie de lancement. Regardez ma vidéo sur le [Comet](https://youtu.be/JbgMa_pn0Lo) pour voir comment je l'ai implémenté avec juste quelques [lignes de code](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame_howto.md#How-to-add-a-ball-saver).

L'image ci-dessous montre un prototype APC dans mon Pinbot.

![Pic Pinbot](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/APC_Pinbot.JPG)

Pour voir APC dans ces premières versions, regardez ma vidéo sur le [Black Knight](https://youtu.be/N5ipyHBKzgs)

## Le logiciel

### PinMame

Vous pouvez utiliser  [Lisy](https://lisy.dev/apc.html) pour exécuter PinMame sur la carte APC. Cela vous évite de recoder tout le jeux et vous pouvez exécuter le jeu original.
Pour les systemes 3-7 APC peut être utilisé avec la carte son originale. Dans ces cas c'est une solutions Plug & Play.

Vous pouvez utiliser la carte APC pour générer la partie audio, si par exemple vous n'avez pas la carte son d'origine ou si vous voulez utiliser des sons alternatifs ou si vous avez un system 9-11. Dans ce cas, cela nécessite un peu de travail au niveau de la configuration de PinMame. Regardez cette  [page](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md)pour plus d'informations.

Quand vous faites tourner la rom dur PinMame vous pouvez utiliser les [PinMameExceptions](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameExceptions.md)  pour changer les règles de votre jeux même si il est contrôlé par PinMame.
Regardez la vidéo du  [Jungle Lord](https://www.youtube.com/watch?v=bbfhH_-gMfE)  pour voir un exemple. Le code correspondant est disponibledans le fichier PinMameExceptions.ino dans la branche AmokPrivate sur GitHub.
Un autre exemple, est le "Ball Save" que j'ai ajouté sur le [Comet](https://youtu.be/JbgMa_pn0Lo).

### MPF

Pour ceux que veulent créer leurs propres règles sans utiliser le langage C, APC supporte le [Framework Mission Pinball](http://missionpinball.org/). Il peut exécuter les commandes envoyé par un PC sur le port USB.

J'ai fait une vidéo afin de présenter le fonctionne de base et pour vérifier que tout fonctionne.  [MPF setup](https://github.com/AmokSolderer/APC/tree/master/DOC/Software/MPF) :

[MPF fait tourner APC](https://www.youtube.com/watch?v=w4Po8OE5Zkw)

### C code

Si vous connaissez le langage C, vous pouvez aussi programmer directement votre code sur la carte APC. Le jeux tournera alors sur l'Arduino, il n'y aura donc pas besoin de PC ou Raspberry Pi.

APC fournis une  [API](https://github.com/AmokSolderer/APC/tree/master/DOC/Software/APC_SW_reference.pdf)donnant accès a toutes les commandes nécessaires pour piloter un flipper. Cela demande pas mal d'effort pour coder un jeu de A à Z, sinon vous pouvez exécuter le jeux via PinMame et utiliser cette API pour changer un peu les règles.

Vous n'avez pas besoin de tout coder en partant de zéro. Le [Base Code](https://github.com/AmokSolderer/APC/blob/master/BaseCode.ino)  contient le code nécessaire a pour lancer une partie et gérer les fonctionnalités de base d'un flipper, vous pouvez partir de celui-ci pour coder votre jeu.

Comme mentionné précédemment, [PinMameExceptions](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameExceptions.md)  peut changer radicalement un jeu avec juste quelques lignes de code.

## Hardware

La carte APC est maintenant assez stable. J'ai une version 2.0 depuis Janvier 2018 et tout fonctionne bien avec. Le hardware est sensiblement le même, j'ai juste ajouter de nouvelles fonctionnalités dans les nouvelles versions et j'ai modifier les plans pour pouvoir les produire plus facilement.

Vous pouvez trouver les schémas et la liste du matériel dans la section [Hardware](https://github.com/AmokSolderer/APC/tree/master/DOC/Hardware). Il y a eu quelques changement mineurs expliqué dans le [Changelog](https://github.com/AmokSolderer/APC/tree/master/DOC/Changes.md).

APC propose une interface d'extension qui est un BUS 8 bit. Pour le moment il y a deux cartes d'extension disponibles, mais vous pouvez l'utiliser pour toute sorte d'extensions.

La première est un carte d'extension permettant de Controller des bandeaux de Leds basés sur des WS2812. Pour plus d'informations allez voir la section [APC LED expansion board](https://github.com/AmokSolderer/APC/blob/master/DOC/LEDexpBoard.md).

L'image ci-dessous, montre l'installation APC dans une Comet avec une carte d'extension LED.

![APC Comet LED](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/CometLED.jpg)

Une courte vidéo de cette installation est disponible ici :

[GI LEDs](https://youtu.be/kLWVUdhSwfo)

La deuxième, est une carte de pilotage de [8 solénoïde supplémentaires](https://github.com/AmokSolderer/APC/blob/master/DOC/SolExpBoard.md). La version actuelle est principalement faite pour être utilisée avec une alimentation externe (par exemple une alimentation 24V pour une shaker).

J'utilise des [afficheurs alphanumériques](https://github.com/AmokSolderer/APC/blob/master/DOC/Sys7Alpha.md) spéciaux sur mon Black Knight que l'on peut aussi trouvé dans la section HW comme remplacement des afficheurs d'origine des system 7.


## Statut actuel (Mars 2023)

Le tableau suivant montre un aperçu des différents systèmes sur lesquels APC peut être utilisé avec au moins une machine de chaque génération confirmée comme fonctionnelle. En plus vous verrez si PinMame ou [MPF](http://missionpinball.org/)a été testé sur ces machines également mais aussi si il y a des spécifiés (comme des extensions de câbles). Les détails sont dans cette [section](https://github.com/AmokSolderer/APC/blob/master/DOC/HowToStart.md#cable-extensions).

Le support de PinMame est en cours de développement et si une génération de machine est supportée, vous aurez besoin des fichiers audio pour certains jeux. Une liste des sons disponible et les explications pour extraire ses propres sons est accessible sur cette [page](https://github.com/AmokSolderer/APC/tree/master/DOC/PinMame.md).

| Williams System | Testé  | PinMame support | MPF support | Commentaire |
|--|--|--|--|--|
|3| Pas encore | Pas encore | Oui|  |
|4| Oui | Pas encore | Oui|  |
|6| Oui | Oui | Oui |  |
|7| Oui | Oui | Oui | Nécessite deux câbles additionnels |
|9| Oui | Oui | Oui |  |
|11| Oui | Oui | Oui |  |
|11a| Oui | Oui | Oui | Quelques colliers doivent être coupé pour libérer certains câbles |
|11b| Pas encore | Pas encore | Oui |  |
|11c| Oui | Oui | Oui | Nécessite trois rallonges de câbles |

Les MPU Data East suivant sont presque identiques aux Williams sauf que Data East utilise du son stéréo. Regardez la page [Problèmes connus](https://github.com/AmokSolderer/APC/blob/master/README-FR.md#known-issues) pour plus de détails

| Data East Version | Testé  | PinMame support | MPF support | Commentaire |
|--|--|--|--|--|
|1| Pas encore | Oui | Oui | nécessite une adaptation du câble audio car APC ne fait que du mono |
|2| Pas encore | Oui | Oui | nécessite une adaptation du câble audio car APC ne fait que du mono |
|3| Oui | Oui | Oui | Seulement pour les Alphanumériques / nécessite une adaptation du câble audio car APC ne fait que du mono / nécessite quelques [rallonges de cables](https://pinside.com/pinball/forum/topic/arduino-pinball-controller/page/18#post-6846714)  |

## Evolutions / Quoi de neuf ?

Un historique des évolutions est disponible dans le [Changelog](https://github.com/AmokSolderer/APC/tree/master/DOC/Changes.md)

## Problèmes connus

### Son stéréo

Quelques jeux comme le Jokerz! ou les Data East ont des systèmes stéréo 2.1. Ce n'est pas supporté par le firmware APC actuellement.
Du coté Hardware, APC est capable de générer deux canaux audio indépendants et peut donc générer du son stéréo. Mais actuement un canal est utilisé pour la musique et l'autre pour les sons qui sont mixés par l'amplificateur de sortie.
Pour utiliser ces systèmes avec APC vous devrez fabriquer un cable audio adapté et la sorite sera en mono uniquement.

### Moteurs pas à pas

Certains jeux comme Jokerz! ou Riverboat Gamble utilisent des moteurs pas à pas pour faire tourner une roue dans le fronton et le nombre de pas sert a déterminé à quel endroit se trouve la roue.
Il y a une problème avec Lisy/PinMame qui n'envoie pas tous les pas à l'APC. Cela signifie qu'il est impossible a APC de déterminer la position de la roue.
Veuillez noter que ce n'est pas un souci de la carte APC, mais uniquement de Lisy/PinMame. Si vous faites votre propre code, vous n'aurez aucun souci.oblems.

### Problème de musique sur Rollergames

L'implémentation de Rollergames sur PinMame a un bug dans la gestion de la musique. Pour une raison quelconque une nouvelle piste audio commence chaque secondes. La plupart du temps c'est la même musique que celle en cours. J'ai implémenté un contournement au problème en créant une exception PinMame. Cela améliore la situation mais il faudrait l'améliorer pour vraiment résoudre le souci. Pour le moment seul Rollergames semble avoir ce souci, les autres machines de même génération fonctionnent bien.

## Vos retours

Vos retour sont très importants pour moi, car si il n'y en a pas je penserai que personne n'est intéressé par le projet et je pourrais arrêter de le documenter. J'ai fait mon possible afin de vous aider a vous familiariser avec ce projet, je suis la aussi pour faire un peu de support. Comme je l'ai dit c'est un hobby, alors n'espérez pas du support 24/7 mais je ferais mon possible pour vous aider.

**Si vous êtes intéressés vous pouvez me faire des retour sur [le forum Allemand Flippertreff ](https://www.flippertreff.de/start/forum/topic/11356-arduino-pinball-controller/) ou sur le   - [forum Pinside (Anglais)](https://pinside.com/pinball/forum/topic/arduino-pinball-controller#post-4898318).**

## Par ou commencer ?

Je suis désolé mais je n'ai pas prévu de vendre des cartes. Regardez et demandez sur les forums certains auront surement des cartes à vendre. Si vous n'en trouvez pas vous pouvez en faire fabriquer en Chine.
Si vous vous intéressez à l'utilisation d'APC, assurez vous de suivre les instructions données dans la première partie de la documentation ci-dessous. Elle va vous guider dans le process de configuration de votre carte APC.

## Sommaire de la documentation

1. Fabriquer et configurer sa carte APC
1.1. [La carte APC](https://github.com/AmokSolderer/APC/blob/master/DOC-FR/APC_board.md) - Comment en acquérir une et la préparer  
1.2. [Installer le logiciel](https://github.com/AmokSolderer/APC/blob/master/DOC-FR/Upload_SW.md) - Comment programmer l'Arduino DUE
1.3. [Présentation basique](https://github.com/AmokSolderer/APC/blob/master/DOC-FR/Prepare.md) -Matériel et cables requis 
1.4. [Effectuer les premiers tests](https://github.com/AmokSolderer/APC/blob/master/DOC-FR/InitialTests.md) - Tester votre carte  
1.5. [Exécuter un jeu](https://github.com/AmokSolderer/APC/blob/master/DOC-FR/RunGame.md) - Toute les information pour lancer un jeu  

2. Réferences  
2.1. [Configurer le BaseCode](https://github.com/AmokSolderer/APC/blob/master/DOC/SetUpBC.md) - Utiliser le BaseCode afin de lancer un jeu basique
2.2. [Outils utiles](https://github.com/AmokSolderer/APC/blob/master/DOC/UsefulSWtools.md) - Outils pour la conversion audio
2.3. [Paramètres APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md) - Les paramètres APC et leur utilité
2.4. [Les schémas APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Hardware/APC_schematics.pdf) - Juste au cas ou vous voulez savoir comment ça fonctionne.
2.5. [Si quelque chose ne fonctionne pas](https://github.com/AmokSolderer/APC/blob/master/DOC/Problems.md) - Si vous avez un problème, lisez ceci en premier

3. Ecfire son propre jeu. Vous voulez programmer votre jeu ? Vous voulez coder en C ? Alors, lisez ça.  
3.1. [Tutoriel pour coder son jeu](https://github.com/AmokSolderer/APC/blob/master/DOC/GameCodeTutorial.md) - Des instruction pas à pas pour écrire votre code de jeu  
3.2. [Référence de code APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/APC_SW_reference.pdf) - Toutes les commandes de l'API d'APC

4. Exécuter PinMame  
4.1. [Site de Lisy](https://lisy.dev/apc.html) - Site pour télécharger Lisy ainsi que des informations complémentaires
4.2. [PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md) - Explication du status acuel de PinMame avec APC
4.3. [Tutoriel audio PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame_howto.md) - Si votre jeu n'est pas encore supporté, vous pouvez changer ça  
4.4. [Les execptions PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameExceptions.md) - changez votre jeu mais laissez PinMame faire le plus gros
4.5. [Liste des jeux PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/lisyminigames.csv) - liste des jeu pinmame avec leur numéro
4.6. [Controler Lisy](https://github.com/AmokSolderer/APC/blob/master/DOC/LisyDebug.md) - mettre à jour Lisy et utiliser le mode debug  
4.7. [Instructions pour extraire les fichiers sons](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameSounds.md) - extraire les sons et les transofrmer avec Audacity (by Mokopin)

5. Utiliser MPF 
5.1. [MPF et APC](https://www.youtube.com/watch?v=w4Po8OE5Zkw) - Ma prmière vidéo sur MPF  
5.2. [MPF configuration](https://github.com/AmokSolderer/APC/tree/master/DOC/Software/MPF) - mes fichiers de configuration pour MPF
5.3. Lisy et MPF

6.  spécifique APC- cartes d'extensions pour APC
6.1. [APC LED extension](https://github.com/AmokSolderer/APC/blob/master/DOC/LEDexpBoard.md) - une carte poru controller les nabdeaux de led WS2812
6.2. [APC solenoid extension ](https://github.com/AmokSolderer/APC/blob/master/DOC/SolExpBoard.md) - pour controller des solénoides suplémentaires  
6.3. [Affichange Alphanumérique pour System 7](https://github.com/AmokSolderer/APC/blob/master/DOC/Sys7Alpha.md) - pour avoir des affichage alphanu sur les machines avant les system 11

7. Hardware aditionnels qui peut être utilisé sans APC - Juste quelques design que j'ai fait
7.1. [Affichange System 7 LED](https://github.com/AmokSolderer/APC/tree/master/DOC/Hardware/Sys7_Display) - un afficheur LED de remplacement pour les System 7 (numérique seulement)
7.2. [Affichage System 11a LED](https://github.com/AmokSolderer/APC/tree/master/DOC/Hardware/Sys11a_Display) - un afficheur LED de remplacement pour les System 11a

8. Jeux APC - Des règles de jeux complets fonctonnant nattivement avec APC ou des jeux utilisant PinMame avec quelques règles alternatives
8.1. [Black Knight](https://github.com/AmokSolderer/APC/blob/master/DOC/BlackKnight.md) - Jeu complet avec des fonctionnalités suplémentaires
8.2. [Pin Bot](https://github.com/AmokSolderer/APC/blob/master/DOC/Pinbot.md) - Jeu complet avec des fonctionnalités suplémentaires
8.3. [Comet](https://github.com/AmokSolderer/APC/blob/master/DOC/Comet.md) - Des ajouts au jeu original exécuté par PinMame

9. Vidéos - Pour la génération Youtube (vidéos en Anglais)
9.1. [Quoi de neuf sur APC3](https://youtu.be/4EgOTJyxMXo) - Donne un aperçu de ce qu'est al carte APC en version 3 et ce qu'elle peut faire
9.2. [Lisy, APC et PinMame](https://youtu.be/cXrh-XPqCKw) - Montre l'utilisation de PinMame avec APC  
9.3. [PinMameExceptions](https://youtu.be/bbfhH_-gMfE) - Qu'est ce que les execptions PinMame et comment elles fonctionnent  
9.4. [Ajouter une "BallSave" avec PinMameExceptions](https://youtu.be/JbgMa_pn0Lo) - Un autre exemple d'utilisation des exceptions PinMame  
9.5. [APC MPF](https://youtu.be/w4Po8OE5Zkw) - Une simple démonstration d'APC controllé par MPF
9.6. [GI LEDs](https://youtu.be/kLWVUdhSwfo) - Montre la carte d'extensuion LED en action
9.7. [Arduino Pinball Controller extension](https://youtu.be/8BnVTpKq-2Y) - Montre aussi la carte d'extensuion LED en action à ses premières versions  
9.8. [APC Black Knight demo](https://youtu.be/N5ipyHBKzgs) - Ma première vidéo d'APC. Pas vraiment à jour mais montre les bases
9.9. [Comment utiliser un afficheur numérique](https://youtu.be/2A5Tt9FQ2as) - Coment naviguer dans les menus
9.10. [Comment paramètre un afficheur 2x16 alphanumerique](https://youtu.be/XqPWbm-HWM8) - Comment trouver le bon paramètre pour sélection le bon afficheur

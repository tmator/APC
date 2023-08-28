# Préparation de base

## carte SD

L'APC a besoin d'une carte SD pour ses paramètres ainsi que pour les fichiers audio.

À partir de la version 3.1, l'APC dispose d'un emplacement pour carte SD intégré, situé sous l'Arduino.

![SD slot](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/SDonBoard.JPG)

Si vous souhaitez que l'APC génère le son, vous devriez reformater votre carte SD avant de l'utiliser, de préférence avec l'outil fourni par l'Association SD (sdcard.org). Cela peut améliorer le temps d'accès aux fichiers de la carte, ce qui est crucial pour que le système audio fonctionne correctement.

### Adaptateur SD fait maison

Selon votre machine, l'accès à l'emplacement de la carte SD intégrée peut être fastidieux. Vous voudrez peut-être donc fabriquer un adaptateur simple qui peut être branché sur le connecteur P8 et qui sera facile d'accés
.
![Adaptateur SD](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/SDadapter.JPG)

Cet adaptateur peut être fabriqué très facilement en utilisant simplement un adaptateur micro SD et en soudant une rangée de broches en dessous. Ensuite, il doit être branché sur le connecteur de carte SD P8 de l'APC. L'orientation doit être telle que l'adaptateur pointe à l'oposé de l'Arduino. Regardez la photo ci-dessous si vous n'êtes pas sûr de l'orientation.

Bien sûr, vous ne pouvez pas utiliser à la fois le logement pour carte SD et l'adaptateur que vous avez fabriqué, car l'APC ne peut utiliser qu'une carte SD à la fois.

# Préparation spécifique suivant la génération 

Cette page est divisée en différentes sections, en fonction de la génération de votre flipper.

## System 3 - 6 

### System 3 - 7 câble blanking pour l'afficheur

Les jeux System 3 - 7 nécessitent un seul fil pour le blanking de l'affichage. Sur le APC, vous pouvez utiliser soit la broche 12 des segments d'affichage 1 (1J5), soit P3, qui est une broche unique située juste à côté de 1J5 et étiquetée Blank_N. Ce signal doit être connecté à la broche 4 de 1P3 (le connecteur qui était branché sur 1J3 de votre ancienne carte CPU). Comme il s'agit de la seule broche fonctionnelle sur 1P3, une façon simple de réaliser la connexion consiste à tourner 1P3 de 90 degrés et à le brancher sur la broche 12 de 1J5 sur le APC. L'image ci-dessous montre cette configuration dans un Jungle Lord System7.

![Sys7DispCable](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Sys7DispCable.JPG)

### System 7 câble pour l'affichage des virgules

L'image ci-dessus montre également les deux connexions supplémentaires à réaliser pour que les virgules des affichages Sys7 fonctionnent. L'APC les a sur les broches 10 (virgule 1+2) et 11 (virgule 3+4) de 1J5 et sur les broches 3 (virgule 1+2) et 4 (virgule 3+4) de 1J8. L'une de ces connexions doit être effectuée sur les broches 2 (virgule 1+2) et 1 (virgule 3+4) de l'ancien connecteur 1P8.
Sur l'image, la 1J8 du APC a été utilisée pour fournir les signaux.

### System 7 câble audio

Vous pouvez fabriquer un simple câble à 5 fils pour utiliser les cartes audio System7 avec le APC 3.0 (les ancienrs cartes APC nécessitent du matériel supplémentaire). Ce câble doit connecter les broches 3 à 7 de l'interface d'extension HW (P11) du APC 3.0 à la carte audio. Une manière simple de faire cela est de les connecter aux broches 12 à 8 de 1P8 (la fiche correspondant à 1J8 de votre carte CPU Sys7).

|APC pin (P11)| 1P8 pin |
|--|--|
|3|12|
|4|11|
|5|10|
|6|9|
|7|8|

![Sys7SoundCable](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Sys7SoundCable.jpg)

Pour que l'APC utilise cette interface, vous devez régler le paramètre 'PinMame Sound sur 'Board'. Ce paramètre se trouve dans le menu 'Game Settings' en mode 'Remote Control'.

## System 9 - 11

Pour les machines System 9, 11 et 11a, l'APC s'intègre assez bien, vous devrez peut-être couper quelques attaches de câble.
L'utilisation des anciennes cartes audio n'est pas possible pour ces machines.

### Rallonges de câbles pour System11b + c

Pour certains jeux System11, vous devrez rallonger certains câbles pour pouvoir les connecter à l'APC. Pour les cartes System11a, ce n'est pas nécessaire, mais vous devrez peut-être couper quelques attaches de câble pour ouvrir un peu l'ensemble de câbles. Si vous ne souhaitez pas le faire, vous pouvez utiliser des rallonges de câble à la place.

Pour les jeux ultérieurs comportant des cartes d'alimentation auxiliaires et d'interconnexion, il n'y a pas moyen d'éviter cela. Pour une machine System11c comme Rollergames, vous devez étendre les câbles jusqu'au connecteur d'alimentation 1J17, le connecteur de lampe 1J7 et l'un des solénoïdes 1J19. Pour la deuxième partie de 1J19, il suffit de couper une attache de câble. Le résultat ressemble à ceci :

![APC_Rollergames](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/APC_Rollergames.JPG)

# Prêt pour les tests

La prochaine étape consiste à [effectuer les tests](https://github.com/AmokSolderer/APC/blob/master/DOC/InitialTests.md).

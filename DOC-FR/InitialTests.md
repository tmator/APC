# Tests initiaux

## Préparation

Poru ces test vous devez installer tous les composants sauf le Raspberry Pi et le logiciel APC installé.
Pour les tests audi, prenez un son de votre choix, convertissez le avec [l'outil adéquat](https://github.com/AmokSolderer/APC/blob/master/DOC/UsefulSWtools.md), renommez le en MUSIC.BIN et copiez sur la carte SD que vous connecterez sur le port P8 ou dans le slot SD sous l'arduino. Vous pouvez passer cette étape si vous ne souhaitez pas utiliser APC pour la gestion du son.

## Switchs et afficheurs

Installez la carte dans votre flipper, mais branchez uniquement les connecteurs de l'alimenation, des switchs et des afficheurs.

| Nom du connecteur | System 3 - 7 | System 9 + 11 |
|--|--|--|
| Logic Power | 1J2 | 1J17 |
| Switch Drivers | 2J2 | 1J8 |
| Switch Inputs | 2J3 | 1J10 |
| Diag_sw | 1J4 | 1J14 |
| Special Switches | 2J13 | 1J18 (if present) |
| Display Strobes 1 | 1J7 | 1J1 |
| Display Strobes 2 | 1J6 | 1J2 |
| Display Segments 1 | 1J5 | 1J3 |
| Display Segments 2 | - | 1J22 |

Les [câbles supplémentaires pour les afficheurs](https://github.com/AmokSolderer/APC/blob/master/DOC/Prepare.md#system-7) pour les System 3 - 7 doivent aussi être installés.

Maintenant, allumez votre flipper. Après quelques messages, votre afficheur devrait afficher 'USB CONTROL'. Si vous ne voyez que deux digitsd très brillants, éteignez immédiatement votre machine, car dans ce cas, le clignotement de votre affichage ne fonctionne pas.
Si vous avez une machine avant System11 avec des affichages numériques ou un modèle System11 à 2x16 chiffres, vous devez ajuster les paramètres d'affichage pour qu'il fonctionne correctement. Une description de la méthode se trouve dans la [section paramètres](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md).

Même si votre affichage fonctionne dès le départ, vous devriez entrer dans les paramètres et les parcourir pour effectuer un test de base des switchs.

Éteignez votre machine une fois le test terminé.

## Solenoids

Dans cette étape nous allons tester les solénoïdes. Pour cela, vous devez connecter les connecteurs suivants :

| Nom du connecteur  | System 3 - 7 | System 9 + 11 |
|--|--|--|
| Solenoid GND | 2J10 | 1J13 |
| Solenoid Drivers 1 | 2J11 | 1J11 |
| Solenoid Drivers 2 | 2J9 | 1J12 |
| Special Sol Drivers | 2J12 | 1J19 |

Si vous entendez une bobine s'activer quand vous allumez le flipper, arretez le immédiatement.
Si aucune bobine ne s'active vous pouvez alors rentrer dans le menu 'System Settings' et définir 'Active Game' sur 'Base Code'.  
Selon le flipper, il est possible qu'une solénoïbobine soit périodiquement activée après avoir quitté les paramètres. Ce n'est pas un problème tant que la bobine ne reste pas activée en permanence, et cela sera probablement résolu après que vous ayez [configuré le Code de Base](https://github.com/AmokSolderer/APC/blob/master/DOC/SetUpBC.md) pour votre machine.

Éteignez votre machine une fois le test terminé.

## Lamps and Audio

Maintenant passons aux lampes et au son.


| Nom du connecteur | System 3 - 7 | System 9 + 11 |
|--|--|--|
| Speakers | 10J2 | depends on sound config |
| Lamp B+ | 2J4 | 1J4 |
| Lamp GND | 2J6 | 1J5 |
| Lamp Strobes | 2J5 | 1J7 |
| Lamp Rows | 2J7 | 1J6 |

Allumez votre machine. Comme vous avez une carte SD branchée dans l'APC, le paramètre précédent devrait avoir été enregistré et le Code de Base devrait s'afficher automatiquement. L'attrac mode du code de base fait que les lampes cyclent de 1 à 64, ce qui devrait être visible maintenant.

Quelques mots sur l'audio :
L'APC propose un réglage de volume numérique qui vous permet d'ajuster le volume dans les Paramètres du système (Paramètre 6 'Speaker Volume'). Cela présente l'avantage de ne pas avoir à utiliser les anciens potentiomètres de volume qui sont souvent défectueux après 40 ans. Cependant, le réglage de volume par défaut est à zéro, ce qui signifie que si vous ne connectez pas le connecteur de volume 10J4 (1J16 pour System11), vous devrez ajuster le réglage de volume pour avoir du son.

## Utilisation des tests

Appuyez sur le bouton "Advance" avec le commutateur Up/Down en position basse pour entrer dans le mode de test du Code de Base.
Le nom du test en cours est affiché à l'écran. Pour les affichages numériques, le numéro du test est affiché dans l'affichage du joueur 1. Vous pouvez entrer dans le test en appuyant sur le bouton de démarrage du jeu ou passer au test suivant en appuyant à nouveau sur "Advance". Les tests d'affichage, de bobine et de lampe unique sont automatiquement en cycle, le numéro correspondant est affiché dans l'affichage du joueur 4. Ce cycle peut être arrêté en position basse du bouton Up/Down.
Le tableau suivant montre les tests disponibles et leur numéro, qui est affiché pour les affichages numériques.

| Nuémro du Test | Nom du Test |
|--|--|
| 1 | Display Test |
| 2 | Switch Edges |
| 3 | Coil Test |
| 4 | Single Lamp |
| 5 | All Lamps |
| 6 | Music Test |

## Prochaine étape

Si les tests sont terminés et que votre carte fonctionne correctement, vous êtes prêt à [lancer une partie](https://github.com/AmokSolderer/APC/blob/master/DOC/RunGame.md)
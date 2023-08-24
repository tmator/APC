# Paramétrer le BaseCode

Pour activer le BaseCode, vous devez le sélectionner dans les paramètre systèmes (Setting 1 'Active Game' should be set to 0 'Base Code'). Allez voir la [page des paramètres](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md) pour savoir comment le configurer.

Pour que le BaseCode fonctionne correctement, vous devez en premier lieu le paramétrer en fonction de votre machine. 
Habituellement cela se fait dans le code lui même, dans le BaseCode c'est implémenté dans les "GamesSettings".
Pour y arriver, appuyez sur le bouton Advance tout en exécutant le code de base, puis entrez dans les "Paramètres du jeu (Games Settings)" qui sont indiqués ci-dessous :

| Numéro | Texte  | Item Nr | Item Text | Default | Comment |
|--|--|--|--|--|--|
| 0 | Outhole Switch | - | - | 9 | Entier - Numéro du switch Outhole |
| 1 | Ball Thru 1 | - | - | 13 | Entier - Numéro du switch 1st Ball Through  |
| 2 | Ball Thru 2 | - | - | 12 | Entier - Numéro du switch  2nd Ball Through, si présent |
| 3 | Ball Thru 3 | - | - | 11 | Entier - Numéro du switch  3rd Ball Through, si présent |
| 4 | Plunger Ln Sw | - | - | 20 | Entier - Numéro du switch de la rampe de lancement |
| 5 | AC Sel Relay | - | - | 12 | Entier - Numéro du relai A/C, mettre a zéro si < system11 |
| 6 | Outhole Kicker | - | - | 1 | Entier - Numéro de la bobine Outhole Kicker |
| 7 | Shooter Ln Feed | - | - | 2 | Entier - Numéro de la bobine Shooter Lane Feeder |
| 8 | Instald Balls | - | - | 3 | Entier - Nombre de balles présente |
| 9 | RestoreDefault | - | - | - | Rien - Restore les paramètres par défaut |
| 10 | Exit Settngs | - | - | - | Rien - Quitte les paramètres et enregistre les paramètres sur la carte SD (si présente) |

Vous devez rechercher ces valeurs dans le manuel de votre flipper. 
Les valeurs par défaut sont valide pour le RollerGames, vous trouverez ci-dessous les pages du manuel d'où son tirées les informations.

![RollergamesSwitches](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/RG_Sw.JPG)

![RollergamesSolenoids](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/RG_Sol.JPG)

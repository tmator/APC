# Paramètres APC

## Selecting le bon afficheur

Le paramètre d'affichage prédéfini est configuré pour les afficheurs des premiers System11. Cela signifie que si vous avez un affichage pré-System11 (chiffres uniquement) ou un Black Knight 2000 (BK2K / 2 x 16 alphanumérique), vous devez d'abord ajuster le paramètre d'affichage. Pour y accéder à l'aveuglette, le réglage de l'afficheur est le premier.

Pour y accéder, vous devez maintenir la touche "Advance" enfoncée pendant plus de 1 seconde avec la touche "Up/Down" en position haute. Attendez 4 secondes, puis appuyez lentement et à plusieurs reprises sur le bouton "Game Start" jusqu'à ce que vous puissiez lire le contenu de l'affichage. Les afficheurs pré-System11 ne peuvent pas afficher de texte, donc pour les affichages à 6 chiffres (Sys3 - 6), un seul 7 est affiché sur l'affichage du joueur 4. Pour les afficheurs à 7 chiffres (Sys7 et 9), c'est un 8.

Une fois que le bon afficheur est sélectionné, appuyez sur "Up/Down" pour le faire descendre et sur "Game Start" pour revenir à un paramètre précédent, puis "Exit Settings". Relâchez ensuite le bouton "Up/Down" et appuyez à nouveau sur "Game Start" pour enregistrer les paramètres et revenir a l'attrac mode.
 
La réinitialisation complète de l'affichage est effectuée lorsque vous quittez les paramètres système. Cela signifie que votre rangée inférieure peut encore sembler un peu étrange même si vous avez sélectionné le bon afficheur jusqu'à ce que vous sortiez des paramètres.

J'ai réalisé deux vidéos pour montrer ce que j'ai décrit ci-dessus :

[Comment utilise les afficheurs numériques](https://www.youtube.com/watch?v=2A5Tt9FQ2as)

[AConfigurer un afficheur 2x16 segments System11](https://www.youtube.com/watch?v=XqPWbm-HWM8)

Notez que les menus ont changé depuis la création de ces vidéos. Utilisez-les donc uniquement pour comprendre comment cela fonctionne en général, mais utilisez les tableaux ci-dessous comme référence pour les paramètres.

## Utilisation du menu des paramètres

Pour entrer dans les paramètres de l'APC, vous devez appuyer sur le bouton "Advance" à la porte du monnayeur. Si vous êtes en mode "Remote control", vous devez le maintenir enfoncé pendant plus de 1 seconde.

Dans le menu de réglage, utilisez le bouton "Advance" pour sélectionner le paramètre et le bouton "Start" pour changer sa valeur. Si le bouton "Up/Down" est en position basse, vous vous déplacez en arrière.

Si une carte SD est présente, les paramètres sont enregistrés lorsque vous choisissez "Exit Settings" (quitter les paramètres).

Les paramètres de l'APC sont divisés en deux parties.

* Les paramètres système sont valables pour tous les jeux actifs sélectionnés
* Les paramètres du jeu sont spécifiques au jeu. Ils changent en fonction du jeu actif 'Active Galme' sélectionné dans les paramètres système

Les jeux avec un afficheur de crédits montrent 00 pour les paramètres système et 01 pour les paramètres de jeu dans les deux premiers chiffres de l'afficheur des crédits. Le numéro du paramètre actuellement affiché apparaît dans les deux derniers chiffres de l'afficheur des crédits. C'est important pour les afficheurs pré-System11 car ils ne peuvent afficher que des chiffres et aucun texte dans le menu.

Pour les afficheurs pré-Sys11, tous les paramètres de texte sont représentés par leur numéro d'élément tel qu'il apparaît dans le tableau. Par exemple, si un afficheur pré-Sys11 est sélectionné et que l'afficheur des crédits montre 00 01, vous avez sélectionné les paramètres système et le paramètre "Active Game" est actuellement affiché. Un afficheur System11 précoce (affichage supérieur alphanumérique et affichage inférieur numérique) tentera toujours d'afficher le nom du jeu actuellement sélectionné sous forme de texte, ce qui n'est pas parfait, mais suffisant. Un afficheur pré-System11 affichera à la place le numéro d'élément et s'il y a un 3 affiché dans l'affichage du joueur 4, vous êtes en mode 'Remote Control'.

### Paramètres dans un jeu PinMame

Lorsque vous exécutez PinMame, vous pouvez ajuster les paramètres d'origine de Williams comme d'habitude, à une exception près :
Vous ne devez pas maintenir le bouton "Advance" enfoncé pendant plus de 1 seconde avec le bouton "Up/Down" en position haute, car cela déclenchera les paramètres de l'APC. Si vous souhaitez parcourir rapidement les paramètres de Williams, faites-le à l'envers avec le bouton "Up/Down" en position basse.

## Paramètres système

| Number | Text  | Item Nr | Item Text | Default | Commentaire |
|--|--|--|--|--|--|
| 0 | Display Type | 0 | 4 Alpha+Credit | X | 4x 7 digit alphanumeric + credit |
| 0 |  | 1 | Sys11 Pinbot | - | 2x 7 digit alphanumeric + 2x 7 digit numeric + credit |
| 0 |  | 2 | Sys11 F-14 | - | 2x 7 digit alphanumeric + 2x 7 digit numeric |
| 0 |  | 3 | Sys11 BK2K | - | 2x 16 digit alphanumeric (inverted segments)|
| 0 |  | 4 | Sys11 Taxi | - | 2x 16 digit alphanumeric + 1x 7 digit numeric (non inverted segments)|
| 0 |  | 5 | Sys11 Riverboat | - | 2x 16 digit alphanumeric + 2x 7 digit numeric (inverted segments)|
| 0 |  | 6 | Data East 2x16 | - | 2x 16 digit alphanumeric (non inverted segments)|
| 0 |  | 7 | 7 | - | 4x 6 digit numeric + credit (System 3 - 6)|
| 0 |  | 8 | 8 | - | 4x 7 digit numeric + credit (System 7 + 9)|
| 1 | Active Game | 0 | Base Code | - | The very basics of a game SW |
| 1 |  | 1 | Black Knight | - | My own Black Knight game SW |
| 1 |  | 2 | Pinbot | - | My own Pinbot game SW |
| 1 |  | 3 | Remote Control | X | For controlling the APC remotely via USB or an onboard Raspberry Pi|
| 1 |  | 4 | Tutorial | - | The corresponding game SW to the game code tutorial Wiki|
| 2 | No of balls | - | - | 3 | Numerical setting - range 1 -5 |
| 3 | Free game | - | - | Yes | Bool setting |
| 4 | Connect Type | 0| Off | - | No remote control during Remote Control mode |
| 4 |  | 1 | On board | - | APC is controlled by the Pi on board |
| 4 |  | 2 | USB | X | APC is controlled via USB |
| 5 | Dim inserts | - | - | No | Bool setting - brightness of playfield lamps is set to 50% when on |
| 6 | Speaker volume | - | - | 0 | Numerical setting - range 1 - 255 / must be set to 0 when volume pot is connected at 10J4 / 1J16 |
| 7 | LED lamps | 0 | No LEDs | X | The APC_LED_exp board is not used |
| 7 |  | 1 | Additional | - | The APC_LED_exp board is used for the lamps 65+ |
| 7 |  | 2 | Playfld only | - | The APC_LED_exp board is only used for the lamps 9 - 64 |
| 7 |  | 3 | PlayfldBackbox | - | The APC_LED_exp board is used for the lamps 1 - 64 |
| 8 | No of LEDs | - | - | 64 | Numerical setting - range 1 - 192 / The length of the LED stripe. Setting is only effective when 'Additional' is selected as 'LED lamps' setting|
| 9 | Sol Exp Board | - | - | No | Bool setting - will use the solenoid expander board for solenoids 26 - 33 if set
| 10 | Debug Mode | - | - | No | Bool setting - Active debug mode will show the number of active timers in the credit display and will stop the game on error |
| 11 | Backbox Lamps | 0| Column 1 | X | Backbox lamps are in lamp column 1 |
| 11 |  | 1 | Column 8 | - | Backbox lamps are in lamp column 8 |
| 11 |  | 2 | None | - | game has no controlled backbox lamps |
| 12 | Restore Default | - | - | - | No setting - restores the default settings |
| 13 | Exit Settings | - | - | - | No setting - exits the settings mode and writes the new setting to an SD card if present |

## Paramètre de jeu en 'Remote Control'

| Number | Text  | Item Nr | Item Text | Default | Commentaire |
|--|--|--|--|--|--|
| 0 | USB watchdog | - | - | No | Bool settings - disables all solenoids when no watchdog reset command is received for 1s |
| 1 | Debug Mode | 0 | Off | X | No debugging |
| 1 |  | 1 | USB | - | Shows the received commands in the displays |
| 1 |  | 2 | Audio | - | Audio debug mode for PinMame Sounds |
| 2 | PinMame Sound | 0 | APC | X | PinMame sounds are played on the APC sound HW |
| 2 | | 1 | Board | - | PinMame sounds are played on an external audio board |
| 3 | PinMame game | - | - | 0 | Numerical setting - PinMame game number |
| 4 | Lisy Debug | - | - | 0 | Numerical setting according to the [Controlling Lisy](https://github.com/AmokSolderer/APC/blob/master/DOC/LisyDebug.md) page |
| 5 - 45 | Setting Unused | - | – | - | They behave like boolean settings, but they have no effect |
| 46 | System3 Set 1 | - | - | - | Use this to change the 1st setting of system 3 games |
| 47 | System3 Set 2 | - | - | - | Use this to change the 2nd setting of system 3 games |
| 48 | System3 Set 3 | - | - | - | Use this to change the 3rd setting of system 3 games |
| 49 | System3 Set 4 | - | - | - | Use this to change the 4th setting of system 3 games |
| 50 | System3 Set 5 | - | - | - | Use this to change the 5th setting of system 3 games |
| 51 | System3 Set 6 | - | - | - | Use this to change the 6th setting of system 3 games |
| 52 | System3 Set 7 | - | - | - | Use this to change the 7th setting of system 3 games |
| 53 | System3 Set 8 | - | - | - | Use this to change the 8th setting of system 3 games |
| 54 | System3 Set 9 | - | - | - | Use this to change the 9th setting of system 3 games |
| 55 | System3 Set 10 | - | - | - | Use this to change the 10th setting of system 3 games |
| 56 | System3 Set 11 | - | - | - | Use this to change the 11th setting of system 3 games |
| 57 | System3 Set 12 | - | - | - | Use this to change the 12th setting of system 3 games |
| 58 | System3 Set 13 | - | - | - | Use this to change the 13th setting of system 3 games |
| 59 | System3 Set 14 | - | - | - | Use this to change the 14th setting of system 3 games |
| 60 | System3 Set 15 | - | - | - | Use this to change the 15th setting of system 3 games |
| 61 | System3 Set 16 | - | - | - | Use this to change the 16th setting of system 3 games |
| 62 | System3 Set 17 | - | - | - | Use this to change the 17th setting of system 3 games |
| 63 | System3 Set 18 | - | - | - | Use this to change the 18th setting of system 3 games |
| 64 | Restore Default | - | - | - | No setting - restores the default settings |
| 65 | Exit Settings | - | - | - | No setting - exits the settings mode and writes the new setting to an SD card if present |

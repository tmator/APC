# Lancer un jeu

Vous avez effectué les tests et le Code de Base fonctionne. Si vous avez également [ajusté le Code de Base](https://github.com/AmokSolderer/APC/blob/master/DOC/SetUpBC.md pour votre machine, vous pouvez déjà appuyer sur le bouton de démarrage du jeu et jouer à un jeu très basique.

Après avoir suffisamment profité du Code de Base, vous avez les options suivantes :

* Utilisez Lisy/PinMame pour exécuter le code de jeu original sur un Raspberry Pi
* Créez votre propre jeu avec MPF et laissez-le contrôler l'APC
* Créez votre propre code de jeu en C et utilisez le cadre de logiciel de l'APC

## PinMame

L'APC en lui-même ne peut pas exécuter le logiciel EPROM original de Williams, mais vous pouvez utiliser PinMame pour émuler le matériel du flipper. Ainsi, vous pouvez exécuter l'ancien logiciel de jeu sur l'APC. La manière la plus simple de faire cela est de brancher un Raspberry Pi sur la carte APC et d'y installer Lisy.
Voici ce que vous devez faire :

1. Entrer dans le menu[System Settings](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md#system-settings)  
1.1. Selectionner 'Remote Control' comme 'Active Game'  
1.2. Vérifier que 'On Board' est sélectionné dans le paramètre 'Connect Type'  
2. Entrer dans les['Game Settings' du mode 'Remote Control'](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md#game-settings-in-remote-control-mode)  
2.1. Selectionne le [numéro du jeu PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/lisyminigames.csv) dans les 'Game Settings'  
2.2. Si vous souhaitez que l'APC génère l'audio, le paramètre de jeu 'PinMame Sound' doit être réglé sur APC, 'Board' signifie qu'une ancienne carte audio est utilisée, ce qui est possible pour les machines de type System 3 - 7. Les machines System 7 nécessitent un câble supplémentaire pour que cela fonctionne  
2.3. Quittez les paramètre et éteignez le flipper  
3. Allez sur la page du [projet Lisy](https://lisy.dev/apc.html) et téléchargez le fichier image. Notez que seuls les modèles Raspberry Pi Zero, Zero W, Pi 2W, 3A+, 3B+ ainsi que le modèle Banana PI M2 Zero sont actuellement pris en charge par Lisy  
3.1. Installez le fichier d'image sur une carte SD et insérez-la dans votre Raspberry Pi  
3.2. Installez le Raspberry Pi sur votre carte APC  
4. Allumez votre flipper  
4.1. Le message 'Booting Lisy' devrait apparaître sur les afficheurs  
4.2. Après un moment, la LED jaune à bord s'allumera et le nom du jeu sélectionné apparaîtra sur les afficheurs ainsi qu'un compte à rebours  
4.3. La LED verte s'allumera et le jeu devrait commencer

La plupart des jeux Williams passent en mode 'Factory Settings' lorsqu'ils sont démarrés pour la première fois. Cela signifie que si le jeu ne démarre pas, utilisez les boutons 'Advance' et 'Up/Down' pour naviguer et quitter les paramètres Williams. Notez que vous êtes maintenant dans les paramètres d'usine Williams et que vous pouvez les naviguer comme d'habitude, à une exception près :
Vous ne devez pas maintenir le bouton 'Advance' enfoncé pendant plus d'1 seconde avec 'Up/Down' en position haute, car cela déclenchera les paramètres APC. Si vous souhaitez parcourir rapidement les paramètres Williams, faites-le simplement à l'envers avec 'Up/Down' en position basse.

Si vous avez l'impression que votre jeu ne fonctionne pas à la vitesse correcte, vous pouvez modifier la vitesse d'émulation de PinMame. Pour ce faire, vous devez retirer la carte SD de votre Raspberry Pi et accéder au fichier
boot/lisy/lisy_m/cfg/lisyminigames.csv
Une valeur de "throttle" est spécifiée pour chaque jeu. Changer cette valeur pour une valeur inférieure fera fonctionner le jeu plus rapidement, et vice versa.


### Le son et PinMame

Quand vous n'utilisez pas la carte son d'origine (sur les modèles ou c'est possible) et que vous souhaitez gérer le son via la carte APC, vous devez disposer des fichiers son. Regardez le tableur sur la [page PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md) afin de savoir si le pack de son est disponible pour votre flipper. Si il est listé vous n'avez qu'a demander les fichiers et les placer sur la carte SD (pas celle du Raspberry Pi).
Si il est pas encore supporté ou que vous souhaitez changer le son (ou même les règles) alors lisez la [documentation sur PinMame](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame_howto.md).

## MPF


Si vous souhaitez créer vos propres règles sans avoir à programmer en C, le [Framework MPF](http://missionpinball.org/) est un bon choix. Il peut s'exécuter sur un PC qui contrôle ensuite l'APC via USB. Vous devez sélectionner 'Remote Control' en tant que jeu actif et 'Connect Type' sur 'USB' dans le [Paramètres systèmes](https://github.com/AmokSolderer/APC/blob/master/DOC/Settings.md#system-settings).  
Nous pouvons également faire en sorte que Lisy exécute MPF, ce qui fonctionnerait sans PC, mais nous le ferions uniquement sur demande.

Vous pouvez trouver ma configuration de base de [MPF ici](https://github.com/AmokSolderer/APC/tree/master/DOC/Software/MPF)

## C code

If you're familiar with C you can also program the APC directly. This SW would then run on the Arduino itself with no need for an additional PC or Raspberry Pi.  
For this the APC software offers an [API](https://github.com/AmokSolderer/APC/tree/master/DOC/Software/APC_SW_reference.pdf) providing the necessary commands to control a pinball machine. It's still a lot of effort to program a game completely from scratch, but you could even run your game in PinMame and only use the API to do changes or extensions to the original rules.

The APC software itself consists of two parts: the operating system APC.ino and the game specific code. The former controls the hardware and offers an application interface (API) for the game specific software to use. For an overview of the available API variables and commands please take a look at the
[APC software reference](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/APC_SW_reference.pdf).

I have written game codes for my Black Knight and Pinbot. They are still not final, but good enough to have fun with and to use as a reference when writing own code. Additionally there's a [Base Code](https://github.com/AmokSolderer/APC/blob/master/BaseCode.ino) which should serve as a starting point for you to do your own game. It contains the very basics of a pinball game and it can be easily adapted to your machine. As a startup guide how to start writing game code I have written a short [tutorial](https://github.com/AmokSolderer/APC/blob/master/DOC/GameCodeTutorial.md).

Please note that I have equipped my Black Knight with a [special alphanumerical display](https://github.com/AmokSolderer/APC/blob/master/DOC/Sys7Alpha.md) for pre System11 machines and that advanced APC commands like scrolling are currently not usable with numerical only displays. This is because I think that these displays are not suited for homebrew machines. If you do all the work needed to do your own game code, you'd for sure want to have a display with letters, otherwise you wouldn't be able to even have a decent high score list. Additionally it would be quite cumbersome to debug some game software without the display being able to show letters. Therfore I recommend to use an early System11 display which has at least one row with alphanumeric displays (or build my [System7Alpha](https://github.com/AmokSolderer/APC/tree/master/DOC/Hardware/Sys7Alpha)). However, the basic software support is implemented, which means you can use the old displays without any restrictions you just have to do a bit more coding to get all the features. And if you just want to use them with PinMame to replace your old boards these displays will work perfectly well.
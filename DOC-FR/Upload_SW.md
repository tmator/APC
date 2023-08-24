# Téléverser le logiciel

## Installer l'Arduino IDE

Il faut utiliser l'Arduino DUE pour programmer la carte APC.
Rendez-vous sur la page [Arduino DUE](https://docs.arduino.cc/hardware/due et utilisez le 'Guide de démarrage rapide' pour voir comment faire.
 
La prochaine étaps consite à installer les librairies SPI et SdFat dans l'IDE Arduino. Il y a un tutoriel sur la [page Arduino](https://docs.arduino.cc/software/ide-v2/tutorials/ide-v2-installing-a-library).

## Télécharger le programme APC depuis GitHub

Si vous ne souhaitez pas utiliser Git pour accéder au dépôt GitHub, vous pouvez plutôt télécharger un fichier ZIP.
Pour ce faire, rendez-vous sur la page principale du projet sur [GitHub ](https://github.com/AmokSolderer/APC) et cliquez sur le bouton vert <> Code en haut à droite de la page. Un petit menu déroulant apparaît avec 'Download ZIP' en tant que dernière entrée. Cliquez dessus pour télécharger le dépôt complet.
Décompressez le fichier et renommez le dossier APC-master en APC. Le dossier doit contenir tous les fichiers .ino et le fichier Sound.h du dépôt APC.

## Téléverser APC

Ouvrez le dossier APC et ouvrez le fichier Arduino.ino dans l'IDE Arduino. Les autres fichiers .ino devraient s'ouvrir sous forme de d'onglets séparés.

Je vous recommande de programmer initialement l'Arduino avant d'installer l'APC dans votre machine. La consommation d'énergie de l'APC est assez faible, donc la connexion USB à votre PC suffit pour l'alimenter.
Connectez le 'Programming port' de l'Arduino DUE à votre PC et appuyez sur 'Téléverser' dans l'IDE Arduino. Le logiciel devrait être compilé et téléversé automatiquement.

Vous êtes maintenant prêt pour la [preparation de base](https://github.com/AmokSolderer/APC/blob/master/DOC/Prepare.md) qui pourrait être nécessaire pour que votre machine fonctionne avec l'APC.

## Mettre à jour le logiciel

Une fois que vous avez installé l'APC dans votre machine, vous n'avez pas besoin de le retirer pour télécharger une nouvelle version du logiciel. Cependant, je vous recommande d'appuyer sur le bouton 'High Score Reset' avant de téléverser une nouvelle version, car cela désactive les lampes, les bobines et les afficheurs.

Après que le téléversement soit terminé, l'APC redémarrera avec le nouveau logiciel. Si vous utilisez un Raspberry Pi avec Lisy, vous devrez peut-être redémarrer votre machine pour que le Pi et l'Arduino se synchronisent à nouveau.
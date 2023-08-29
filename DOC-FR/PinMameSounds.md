# Instructions pour extraire les fichiers sons depuis une ROM

Merci à Mokopin pour ces instructions.

Il existe plusieurs façons d'extraire des sons des ROM de flippers :
1) La méthode 'F5' en utilisant [PINMAME and M1 music extractor](https://www.vpforums.org/index.php?app=tutorials&article=54)  
2) La méthode 'F6' en utilisant [PINMAME with patch by lucky1](https://vpuniverse.com/forums/topic/4489-pinmame-altsound-editor/)

La méthode 2) semble préférable car elle fournit automatiquement une liste de fichiers, mais elle ne fonctionne pas pour tout le monde (OS Win7, Win10/11).
Il semble que la version de pinmame > décembre 2020 soit requise. Avec WIN10, cela ne fonctionne pas avec le pinmame32.exe de sourceforge qui est la version 3.3 (29 juillet 2020). Cela fonctionne parfaitement avec la version 3.3b de Toxie (https://www.vpforums.org/index.php?app=downloads&showfile=11571)
La méthode 1) est complexe car il faut appuyer sans arret sur la touche F5 et nécessite donc des nerfs solides pour de nombreux fichiers...

Les deux méthodes ont en commun que la carte son du flipper est déclenchée par des commandes spécifiques à chaque machine (par exemple, 0x0101 signifie jouer la chanson 1 sur la carte son, 0x0001 signifie jouer le son 1 sur la carte son). Avec la méthode 1), un certain nombre de fichiers seront disponibles dans le dossier 'wave' du dossier pinmame (par exemple, 'rdkn0001.wav'). Ces fichiers sont échantillonnés à 48 kHz, qui est la fréquence d'échantillonnage par défaut de pinmame et qui doit être convertie en la fréquence d'échantillonnage de l'APC de 44,1 kHz plus tard.

Avant ça, il est nécessaire de renommer et de post-traiter les fichiers. Cela peut être effectué avec un outil de renommage automatique (par exemple, Bulk Rename Utility https://www.bulkrenameutility.co.uk). Les fichiers audio doivent être modifiés en '00_XX.wav' avec XX en format hexadécimal, et les fichiers musicaux en '01_XX.wav' en conséquence.

La prochaine étape est le post-traitement dans un outil audio comme audacity (http://www.audacity.com). Avec audacity, presque toutes les opérations peuvent être traitées par lots. C'est très pratique pour changer le volume relatif des fichiers musicaux et/ou des fichiers audio d'un simple clic de souris.
Il peut également être utilisé pour automatiser une série chronologique de fichiers son exportés avec la touche 'F5'. Par exemple, si la méthode 'F5' est utilisée pour enregistrer les sons de 0x00 à 0x0F dans un seul fichier son, les sons peuvent être détectés par seuil dans audacity et enregistrés automatiquement.
Cela peut être réalisé en insérant les commandes suivantes dans la file d'attente des macros :

1) Séléxctionner tous les fichiers
2) Filtre passe Haut/Highpass filter 10Hz /6dB
3) noisefinder (-)28db, 0.1 , 0.1 , 0  (la valeur en dB peut nécessiter une adaptation si le résultan'est pas satisfaisant)

Après avoir exécuté la macro, vérifiez si les marqueurs de texte sont correctement définis. Ensuite, utilisez CTRL-Shift-L pour exporter des fichiers uniques définis par les marqueurs de texte (utilisez le préfixe '0_').

L'étape suivante recommandée consiste à passer la piste stéréo en mono en mixant les deux cannaux (utilisez Tracks->mix->downmix to mono dans audacity) et à changer le volume (utilisez la fonction 'normalize' ou 'amplify' dans audacity). Expérimentez un peu avec l'amplification, car les valeurs de crête générées synthétiquement (par pinmame) sont parfois trompeuses. Essayez-le sur le flipper afin de valider le volume.

Après cette étape, vous pouvez soit convertir chaque fichier en 44,1 kHz manuellement dans audacity (pour cela, il n'existe pas encore de fonction de macro). Ou utilisez un autre outil comme OCENAUDIO (https://www.ocenaudio.com) qui peut convertir par lots les fréquences d'échantillonnage. Avec OCENAUDIO, vous pouvez glisser un dossier dans la colonne de gauche de l'écran du programme, tout sélectionner avec CTRL-A et utiliser le bouton droit de la souris pour 'changer la fréquence d'échantillonnage' et la régler à 44100. Fermez ensuite tous les fichiers et confirmez "enregistrer les fichiers" avec OK et attendez la fin de la conversion automatique.

La dernière étape consiste à convertir les fichiers WAV 16 bits en format APC 12 bits. Cela peut être fait avec un script PERL qui prend tous les fichiers WAV (mono) dans le répertoire actuel et les convertit en fichiers *.snd. Ce script peut être exécuté avec un interpréteur perl, par exemple pour Windows disponible sur http://strawberryperl.com

Les fichiers .snd résultants doivent être copiés sur la carte SD de l'APC. Utilisez un utilitaire de formatage de carte SD (par exemple https://www.sdcardformatter.com/) et non la fonction du système d'exploitation. Les systèmes de fichiers FAT ne trient ni n'indexent les fichiers, de sorte que l'ouverture effectue une recherche séquentielle du fichier. L'ouverture d'un fichier prend normalement environ 15 ms, mais peut prendre jusqu'à 50 ms. De plus, le contrôleur de la carte SD peut effectuer certaines "opérations de maintenance", prolongeant encore les temps d'accès. En conséquence, un fichier son peut ne pas être trouvé par l'APC, même s'il est disponible sur la carte SD (avec un plus grand nombre de fichiers stockés dessus).

Les fichiers .snd doivent êtres ensuite copiés sur la carte SD de l'APC. Utilisez un utilitaire de formatage de carte SD (par exemple https://www.sdcardformatter.com/) et non la fonction du système d'exploitation. Les systèmes de fichiers FAT ne trient ni n'indexent les fichiers, de sorte que l'ouverture effectue une recherche séquentielle du fichier. L'ouverture d'un fichier prend normalement environ 15 ms, mais peut prendre jusqu'à 50 ms. De plus, le contrôleur de la carte SD peut effectuer certaines "opérations de maintenance", prolongeant encore les temps d'accès. En conséquence, un fichier son peut ne pas être trouvé par l'APC, même s'il est disponible sur la carte SD (avec un plus grand nombre de fichiers stockés dessus).

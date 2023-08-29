# PinMame

## Premiers pas

Pour utiliser Lisy, vous avez besoin d'un Raspberry Pi. Actuellement, tous les modèles Pi de type zéro et modèle 3 sont pris en charge.
Téléchargez le [Logiciel Lisy](https://lisy.dev/software.html) et installez-le sur la carte SD de votre Raspberry Pi.
Pour exécuter le code de votre jeu, vous avez besoin du numéro de jeu PinMame de votre jeu, qui peut être trouvé [ici](https://github.com/AmokSolderer/APC/blob/master/DOC/lisyminigames.csv).

### APC 3 avec Raspberry Pi sur la carte

Branchez le Pi dans le connecteur J1 de l'APC. Ensuite, allumez votre APC, accédez aux 'System settings' et sélectionnez 'Remote control' comme 'Active Game' et 'On board' comme 'Connect Tpe'.
Maintenant, accédez aux 'Games settings' et définissez le 'PinMame Game' sur le numéro de votre jeu.

Comme la synchronisation entre Lisy et l'APC n'est pas encore finalisée, il peut être nécessaire de redémarrer votre jeu.
Pendant le démarrage, l'affichage doit afficher 'Booting Lisy'. Une fois le démarrage terminé, la LED jaune de l'APC s'allumera, suivie de la LED verte lorsque la connexion à l'APC sera établie. Maintenant, le nom du jeu devrait apparaître sur les afficheurs avec un compte à rebours après lequel l'émulation du jeu démarre.

### Utiliser la carte Lisy_Mini

Allumez votre APC, entrez dans les 'System settings' et sélectionnez 'Remote control' comme 'Active Game' et 'USB' comme Connect Type'. Réglez les commutateurs DIP S2 de la carte Lisy_Mini sur le numéro de votre jeu.

Connectez le connecteur d'alimentation K8 de votre carte Lisy_Mini à l'alimentation 5V de votre flipper. (Vous pouvez également alimenter Lisy_Mini par le port micro USB du Raspberry Pi. Dans ce cas, vous avez besoin d'une alimentation capable de fournir 2A et vous devriez mettre sous tension Lisy après l'APC et connecter le câble USB ensuite.)

Démarrez votre flipper avec Lisy_Mini et l'APC. Les affichages de votre flipper devraient afficher 'USB Control' jusqu'à ce que la connexion entre Lisy et l'APC soit établie.

## Utiliser la carte son d'origine

Les machines System 3 à 7 peuvent être utilisées avec une ancienne carte son. Pour le System 7, un [câble supplémentaire](https://github.com/AmokSolderer/APC/blob/master/DOC/HowToStart.md#system-7-audio-cable) est nécessaire pour connecter la carte audio. Les jeux System 3 à 6 contrôlent les cartes audio via les pilotes de solénoïdes.

Pour que l'APC contrôle la carte audio externe, vous devez entrer dans les paramètres du jeu en mode de télécommande et régler le paramètre 2 (Son de PinMame) de 0 (APC) à 1 (Carte).

## Laissez la carteAPC gérer le son

Ce qui suit est seulement nécessaire si vous n'utilisez pas l'ancienne carte audio. Les raisons peuvent être que vous souhaitez utiliser une machine System 9 - 11, que vous n'avez pas de carte appropriée, que vous souhaitez modifier certains sons ou que vous souhaitez ajouter une piste de musique supplémentaire à votre jeu System 3 - 7.

Pour que l'APC puisse les jouer, tous les sons doivent être présents sur la carte SD. Dans les sections suivantes, vous apprendrez comment faire cela.

### Enregistrer les sons

Si vous souhaitez obtenir les sons originaux de votre machine, vous devez installer PinMame pour Windows, car la version Unix présente des soucis de qualité sonore.

Après avoir lancé PinMame, appuyez sur F4 pour entrer dans le 'Sound Command Mode' et entrez le préfixe et le numéro hexadécimal du son que vous souhaitez enregistrer. Appuyez ensuite sur F5 pour commencer l'enregistrement, sur ESPACE pour lire le son, puis à nouveau sur F5 pour arrêter l'enregistrement. Le son se trouve dans le dossier 'wave' de PinMame.
Pour une raison quelconque, les sons de PinMame ont une fréquence d'échantillonnage de 48 kHz et doivent donc être convertis à la fréquence d'échantillonnage normale de 44,1 kHz. Vous pouvez utiliser un éditeur audio gratuit comme Audacity pour ajuster le temps de début et de fin de votre échantillon sonore, ainsi que la fréquence d'échantillonnage.
Parfois, le volume des fichiers sonores et musicaux générés par PinMame diffère considérablement. Cependant, l'APC n'offre aucun réglage de correction pour faire correspondre les niveaux de volume de ces canaux, ce qui signifie que vous devriez également utiliser Audacity pour ajuster ces niveaux en conséquence.
Mokopin (du forum Flippertreff) a rédigé des [instructions pour extraire les fichiers sons](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameSounds.md) qui expliquent l'extraction automatique de fichiers sonores et l'utilisation plus détaillée d'Audacity.

Comme tous les sons à être joués par l'APC, les fichiers WAV doivent être traités avec mon convertisseur de données audio. Vous pouvez trouver des informations supplémentaires sur cet outil dans la page [Useful Software Tools](https://github.com/AmokSolderer/APC/blob/master/DOC/UsefulSWtools.md).

Pour que l'APC puisse trouver le bon son, tous les sons doivent être nommés avec le préfixe (un seul chiffre), un underscore, le numéro hexadécimal du son et .snd comme extension. Par exemple, le nom du son 0xf1 avec le préfixe 00 serait 0_f1.snd. Tous les fichiers doivent être placés dans le dossier racine de la carte SD de l'APC.

Les commandes audio de System 7 ont 5 bits et peuvent donc sélectionner uniquement 32 (0x20 en hexadécimal) sons différents. Pour une raison quelconque, PinMame semble ajouter 32 à chaque numéro de son. Par conséquent, les noms de fichiers des sons du système 7 doivent être numérotés de 0x21 à 0x40 pour que l'APC puisse trouver le fichier correct.
Ce décalage pourrait être différent pour d'autres générations de systèmes.

### Quesls sons sont requis ?

Lorsque vous partez de zéro, vous devriez jouer à votre flipper avec Lisy en mode de débogage. Veuillez lire la page de [configuration de Lisy](https://github.com/AmokSolderer/APC/blob/master/DOC/LisyDebug.md) pour apprendre comment faire cela.

Lorsque vous avez fini de jouer, appuyez sur le bouton d'arrêt SW1 sur votre carte APC pour qu'il quitte l'émulation et enregistre le fichier journal sur la carte SD. Le fichier sera situé sur la carte SD du Pi dans le dossier lisy/lisy_m/debug.

Dans le fichier lisy_m_debug.txt, la lecture d'un son ressemble à ceci :

    [461.372965][0.000064] LISY_W sound_handler: board:0 0x79 (121)
    [461.372985][0.000020] play soundindex 121 on board 0 
    [461.373005][0.000020] USB_write(3 bytes): 0x32 0x01 0x79

Dans ce cas, le son 0x79 du préfixe (board) 0 doit être joué, ce qui signifie que vous devez enregistrer la commande son 0079 dans PinMame, la convertir et l'écrire sur la carte SD de l'APC sous le nom 0_79.snd.

### Trouver quels sons sont manquants

Après avoir extrait la plupart des fichiers, il arrivera un moment où il ne manquera que quelques fichiers et vous devrez déterminer leurs numéros. Bien sûr, vous pourriez toujours scanner le journal Lisy à la recherche de numéros de son inconnus, mais à ce stade, il pourrait être plus facile d'utiliser le 'Mode de Débogage Audio' de l'APC.
Vous pouvez activer ce mode dans les paramètres du jeu. Pour ce faire, vous devez appuyer sur "Advance" pendant plus d'une seconde pour entrer dans les paramètres. Utilisez "Advance" pour passer des paramètres système aux paramètres du jeu et appuyez sur le bouton de démarrage pour les confirmer. Utilisez "Advance" pour passer aux paramètres de "Debug Mode", puis changez-le en "AUDIO" en appuyant sur le bouton de démarrage. Allez à "Exit Settings" en appuyant sur "Advance" et sur le bouton "Start" pour confirmer.

En mode de débogage audio, le ou les affichages inférieurs sont utilisés pour les informations audio. L'affichage du Joueur 3 (ou la partie gauche de l'affichage inférieur pour les affichages de type BK2K) montre les informations pour le préfixe sonore 00 et l'affichage du Joueur 4 (partie droite de l'affichage inférieur pour BK2K) fait de même pour le préfixe 01. Si le son demandé est trouvé sur la carte SD, son numéro hexadécimal est affiché dans la partie gauche de l'affichage correspondant et le son est joué normalement. Si le fichier sonore est manquant, son numéro hexadécimal est affiché sur la partie droite de l'affichage correspondant, ce qui facilite la recherche des fichiers son manquants.
Comme les affichages d'avant System11 ne peuvent pas afficher de lettres, les numéros de son correspondants sont affichés en valeurs décimales lorsque ce type d'affichage est sélectionné.

### Problèmes de son

La plupart des problèmes de son tels que les retards et les bégaiements sont causés par les performances de la carte SD. Consultez la [section suivante](https://github.com/AmokSolderer/APC/blob/master/DOC/Problems.md) pas pour en savoir plus.

### Erreurs de commande inconnue

Si vous utilisez l'APC avec Lisy/PinMame et que le message d'erreur 'Unknown Command' suivi d'un numéro apparaît sur vos affichages inférieurs, alors vous avez un problème avec votre communication série. C'est un problème grave qui doit être résolu. Vous pouvez apprendre comment faire cela dans la section [suivante](https://github.com/AmokSolderer/APC/blob/master/DOC/Problems.md)

## Programmer des exceptions

L'APC dispose d'une gestion des exceptions spécifiques à chaque machine, ce qui signifie que vous pouvez manipuler votre jeu même s'il fonctionne sous PinMame. Pour activer cela pour votre machine, vous devez ajouter une section spécifique au jeu dans le fichier PinMameExceptions.ino et recompiler le logiciel.
Vous pouvez manipuler les commandes de son, de lampe, de commutateur, d'affichage et de solénoïde. Certaines de ces exceptions sont nécessaires pour faire fonctionner correctement votre machine, tandis que d'autres sont simplement des améliorations ou des modifications modérées des règles.

### Mettre en place des exceptions sonores sur le Jungle Lord

Utilisons Jungle Lord Systeme 7 comme exemple de l'utilisation des exceptions en pratique :

Tout d'abord, nous devons générer une section de code spécifique à Jungle Lord pour gérer toutes les exceptions requises. Il y a un modèle nommé

    byte EX_Blank(byte Type, byte Command)

dans le fichier PinMameExceptions.ino que vous pouvez utiliser comme point de départ. Créez donc une copie de ce modèle et renommez-la en

    byte EX_JungleLord(byte Type, byte Command)

Pour que le système utilise cette section de code, nous devons l'ajouter à la fonction EX_Init qui se trouve à la fin du fichier PinMameExceptions.ino et qui détermine quel code est utilisé pour quelle machine. Au moment où ce document a été créé, Jungle Lord était la première machine à avoir une telle gestion des exceptions, donc il y a seulement une entrée dans EX_Init pour l'instant :

    void EX_Init(byte GameNumber) {
      switch(GameNumber) {
      case 20:                                            // Jungle Lord
        EX_EjectSolenoid = 2;                             // specify eject coil for improved ball release
        USB_SolTimes[20] = 0;                             // allow permanent on state for magna save relais
        USB_SolTimes[21] = 0;
        PinMameException = EX_JungleLord;                 // use exception rules for Jungle Lord
        break;
      default:
        PinMameException = EX_DummyProcess;}}

Tous les autres jeux n'ont pas encore de gestionnaire d'exceptions, donc le pointeur d'exception pointe simplement vers un processus fictif qui ne joue que les sons standard, mais ne connaît aucune exception. Le changement de USB_SolTimes est seulement nécessaire parce que dans cet exemple je veux également améliorer le temps de réaction des aimants de 'Magna Save', et pour cela il faut pouvoir allumer les aimants en permanence. Mais pour l'instant, ignorez cela ainsi que EX_EjectSolenoid.

Jungle Lord utilise certaines [commandes sonores spécifiques au System 7](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMame.md#system-7) que l'APC doit connaître pour que le son fonctionne correctement. Comme le System7 n'utilise qu'un seul canal sonore, ces exceptions doivent être placées dans le cas SoundCommandCh1 de notre programme EX_JungleLord. Pour un jeu System11, vous devrez également préparer des exceptions pour SoundCommandCh2 car ces jeux utilisent les deux canaux sonores.

La première exception concerne la commande sonore 0x26 qui déclenche l'une des quatre phrases dite au hasard. Dans le gestionnaire d'exceptions, cela ressemble à ceci :

    case SoundCommandCh1:                               // sound commands for channel 1
      if (Command == 38){                               // sound command 0x26 - start game
        char FileName[13] = "0_26_000.snd";             // generate base filename
        FileName[7] = 48 + random(4) + 1;               // change the counter according to random number
        PlaySound(52, (char*) FileName);}               // play the corresponding sound file

L'APC s'attend à ce que les fichiers sonores correspondants soient nommés 0_26_00X.snd, où X est égal à un pour le premier fichier, deux pour le deuxième, et ainsi de suite.
D'abord, nous générons le nom de fichier de base "0_26_000.snd", puis nous changeons le 8e caractère de cette chaîne en un nombre aléatoire entre 1 et 4, et nous jouons le fichier son avec une priorité de 52.

La commande sonore spéciale suivante de Jungle Lord est 0x2d. C'est une série de sons en boucle, ce qui signifie qu'elle recommence avec le premier son après que le dernier a été joué. Le code correspondant est le suivant :

    else if (Command == 45){                          // sound command 0x2d - multiball start - sound series
      if (SoundSeries[1] < 31)                        // this sound has 31 tunes
        SoundSeries[1]++;                             // every call of this sound proceeds with next tune
      else
        SoundSeries[1] = 1;                           // start all over again
      char FileName[13] = "0_2d_000.snd";             // generate base filename
      FileName[7] = 48 + (SoundSeries[1] % 10);       // change the 7th character of filename according to current tune
      FileName[6] = 48 + (SoundSeries[1] % 100) / 10; // the same with the 6th character
      PlaySound(51, (char*) FileName);}               // play the sound

Pour cela, nous avons besoin d'une variable supplémentaire, SoundSeries, qui stocke le numéro du morceau en cours de lecture. Cette variable doit être définie en tant que byte statique au début de notre programme EX_JungleLord.
Tout d'abord, on vérifie si le dernier morceau de cette série est en cours de lecture. Si c'est le cas, le numéro de morceau est ramené à un; sinon, il est augmenté de un. Ensuite, le nom de fichier de base est généré, le nouveau numéro de morceau est inséré et le son est joué.

La commande sonore 0x2a est également une série de sons, donc elle est traitée de manière très similaire.

    else if (Command == 42){                          // sound command 0x2a - background sound - sound series
      SoundSeries[1] = 0;                             // reset the multiball start sound
      if (SoundSeries[0] < 29)                        // BG sound has 29 tunes
        SoundSeries[0]++;                             // every call of this sound proceeds with the next tune
      char FileName[13] = "0_2a_000.snd";             // generate base filename
      FileName[7] = 48 + (SoundSeries[0] % 10);       // change the 7th character of filename according to current tune
      FileName[6] = 48 + (SoundSeries[0] % 100) / 10; // the same with the 6th character
      for (byte i=0; i<12; i++) {                     // store the name of this sound
        USB_RepeatSound[i] = FileName[i];}
      QueueNextSound(USB_RepeatSound);                // select this sound to be repeated
      PlaySound(51, (char*) FileName);}               // play the sound

Une différence est que cette série de sons n'est pas une série en boucle, ce qui signifie que le compteur de morceaux ne revient pas à un, mais reste à la valeur la plus élevée jusqu'à ce qu'il soit réinitialisé par la commande d'arrêt de son 0x2c. De plus, cette commande réinitialise le morceau de la série de sons 0x2d (SoundSeries[1] = 0;).
Cependant, la principale différence est que 0x2a est le son d'arrière-plan qui peut être interrompu par d'autres sons, mais qui reprendra ensuite. Dans le logiciel APC, le pointeur Aftersound peut être utilisé à cet effet. Ce pointeur peut être défini sur une routine qui est automatiquement appelée lorsque le son est terminé. Ici, nous appelons QueueNextSound, qui prend comme argument le nom de fichier pointé par USB_RepeatSound. Il définit le pointeur AfterSound sur une routine qui jouera le fichier dont le nom est stocké dans USB_RepeatSound lorsque le fichier son actuel est terminé. La configuration de AfterSound = 0 désactivera ce mécanisme.

Chaque machine System7 a également besoin de la commande d'arrêt de son 0x2c.

    else if (Command == 44) {                         // sound command 0x2c - stop sound
      AfterSound = 0;
      SoundSeries[0] = 0;                             // Reset BG sound
      SoundSeries[1] = 0;                             // reset the multiball start sound
      StopPlayingSound();}

Cette commande définit AfterSound = 0, ce qui empêchera la reprise du son de fond (BG). Ensuite, elle réinitialise les deux séries de sons et arrête la lecture en cours.

Jusqu'à présent, nous n'avons implémenté que les sons qui sont spéciaux pour le Jungle Lord, mais la majorité des sons sont des sons ordinaires. Cela signifie que nous avons besoin d'un gestionnaire par défaut pour tous les numéros de sons qui n'ont pas été couverts par les exceptions ci-dessus.

    else {                                            // standard sound
      char FileName[9] = "0_00.snd";                  // handle standard sound
      if (USB_GenerateFilename(1, Command, FileName)) { // create filename and check whether file is present
        PlaySound(51, (char*) FileName);}}
    return(0);                                        // return number not relevant for sounds

Les noms de fichier de ces sons se composent simplement du numéro de canal, d'un trait de soulignement et du numéro de son en hexadécimal suivi de ".snd". Il existe une routine appelée

    byte USB_GenerateFilename(byte Channel, byte Sound, char* FileName)

qui ajoute le code hexadécimal à un nom de fichier donné et gère les messages d'affichage si le mode de débogage audio est actif. Il renvoie 1 si le fichier audio existe et 0 s'il n'existe pas. Il est donc seulement nécessaire de jouer le son lorsque la valeur de retour a été de 1.

La déclaration return 1; marque la fin du cas SoundCommandCh1.

### Exceptions musicales pour les machines System11

Avec les System 11, les cartes son sont devenues beaucoup plus puissantes. Cela a rendu possible la lecture d'une partition musicale indépendante en plus des effets sonores et de la parole.

La plupart de ces partitions musicales sont composées d'une introduction et d'une partie en boucle qui se répète continuellement. Si vous voulez simplifier les choses, vous pourriez simplement mettre l'introduction et quelques répétitions de la partie en boucle dans un seul fichier. Cela fonctionnera dans la plupart des cas, mais si vous attendez assez longtemps, la musique finira par s'arrêter.
Pour éviter que cela se produise, l'introduction et la partie en boucle doivent être dans des fichiers séparés et une règle d'exception doit gérer la séquence.

Prenons l'exemple de la piste musicale 4 de Pinbot. Dans le cas SoundCommandCh2, le code suivant gère la piste 4 :

    else if (Command == 4) {                          // music track 4
      PlayMusic(50, "1_04.snd");                      // play non looping part of music track
      QueueNextMusic("1_04L.snd");}                   // queue looping part as next music to be played
	  
Lorsque le canal audio 2 reçoit la commande 4, l'introduction 1_04.snd est d'abord jouée, puis la commande QueueNextMusic est utilisée pour mettre en file d'attente la partie en boucle 1_04L.snd à jouer après que l'introduction soit terminée. Tant que la variable AfterMusic n'est pas définie à zéro, la partie en boucle sera répétée automatiquement.

# Plus de PinMameExceptions

Ce ne sont que des exceptions de base que vous devez fournir pour que l'APC puisse utiliser correctement les fichiers audio sur votre carte SD.
Cependant, les PinMameExceptions peuvent faire beaucoup plus que cela, donc consultez [la page des PinMameExceptions](https://github.com/AmokSolderer/APC/blob/master/DOC/PinMameExceptions.md) pour en savoir plus.

# Comment créer mon propre code de jeu ?


Supposons que vous veniez de construire votre propre carte APC et l'ayez installée dans votre flipper. Vous avez probablement testé le BaseCode.ino adapté comme décrit dans [Configuration du BaseCode](https://github.com/AmokSolderer/APC/blob/master/DOC/SetUpBC.md) et maintenant vous êtes impatient d'écrire vos propres règles. Dans cette page, je vais vous expliquer comment le faire. Dans mon cas, le tutoriel est destiné à être exécuté sur un flipper Rollergames, mais vous pouvez l'adapter à d'autres machines ou simplement le lire pour comprendre les principes de l'API APC. Merci de lire [le référentiel APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/APC_SW_reference.pdf) pour connaitre toutes les commandes et variables disponibles.

## 1 Ajouter une nouvelle règle
Pour commencer, on copie le fichier BaseCode.ino et je le renomme en un nouveau fichier .ino dans le dossier APC. Dans mon cas, je crée le fichier Tutorial.ino dans le dossier APC, où se trouvent déjà APC.ino, BlackKnight.ino,... . Ensuite, je change tous les préfixes BC_ en ceux de mon propre jeu pour éviter les erreurs de compilation. Pour le tutoriel, je les change tous en TT_.
### 1.1 La structure GameDefinition
Après avoir changé tous les préfixes, la structure GameDefinition ressemble à ça :

    struct GameDef TT_GameDefinition = {
        TT_setList,                           // GameSettingsList
        (byte*)TT_defaults,                   // GameDefaultsPointer
        "TT_SET.BIN",                         // GameSettingsFileName
        "TT_SCORE.BIN",                       // HighScoresFileName
        TT_AttractMode,                       // AttractMode
        TT_SolTimes};                         // Default activation times of solenoids

Chaque jeu possède son propre fichier de scores et de paramètres sur la carte SD. Les deux noms de fichier (.BIN) indiquent au système quels noms utiliser. Ils peuvent être choisis librement tant qu'ils ne dépassent pas 8 caractères. Les autres variables doivent être définies comme ci-dessus.
### 1.2 Changer le menu
TT_setList est une liste de paramètres. Elle définie l'affichage du menue de paramètres de ma machine et comment ces paramètres seront gérés. Par exemple le 'God Mode" défini dans le BaseCode (Qui ne sert a rien mais qui est la pour l'exemple). Voici à quoi ressemble cette liste :

    struct SettingTopic TT_setList[5] = {{"  GOD   MODE  ",HandleBoolSetting,0,0,0}, // defines the game specific settings
        {" RESET  HIGH  ",TT_ResetHighScores,0,0,0},
        {"RESTOREDEFAULT",RestoreDefaults,0,0,0},
        {"  EXIT SETTNGS",ExitSettings,0,0,0},
        {"",NULL,0,0,0}};

Ce mode activable par les paramètres est défini en tant ue Booléen (pour plus de détails sur la commande SelSetting regardez dans la documentation de l'API).
Les paramètres „Restore Defaults“ et „Exit Settings“ sont implicites et le dernier qui contient des zéros sert a signifier la fin de la liste.
Maintenant que nous avons défini le menu des paramètres, nous avons également besoin des valeurs de paramètres correspondantes. Le logiciel APC utilise un tableau de 64 octets pour stocker ces valeurs :

    byte game_settings[64]; // game settings to be stored on the SD

Quand une carte SD est détectée pendant le démarrage et qu'elle contient un fichier avec le nom spécifié plus haut (TT_SET.BIN) alors le contenu est lu et les paramètres sont chargés dans le tableau game_setting. Si le fichier n'est pas trouvé le pointeur en position 2 de GameDefinition (TT_defaults) est utilisé pour charger les valeurs par défaut. Ces valeurs seront aussi utilisées sis vous choisissez "Restore Default" dans le menu des paramètre du jeu.
Comme je ne souhaite pas avoir le "God Mode" par défaut, je peux laisser le tableau ainsi :

    const byte TT_defaults[64] = {0,0,0,0,0,0,0,0, // game default settings
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0,
     0,0,0,0,0,0,0,0};

Les valeurs par défaut doivent être dans le même ordre que les élément dans TT_setLits.
Si vous voulez en savoir plus sur les paramètres systèmes, allez voir APC_setList dans le fichier APC.ino, car la structure SettingTopic est également utilisée pour les paramètres du système. Cela signifie que vous pouvez retrouver tout ce qui a été mentionné ci-dessus dans le fichier APC.ino avec un contenu réel et le préfixe APC_.
La prochaine étape est de dire au système par ou démarrer le jeu. Comme la plupart des flippers, nous le démarrons en "Attrac Mode", le pointeur correspondant est appelé TT_AttractMode. Pour commencer, je décommente simplement le TT_AttractMode copié à partir du Base Code et le modifie comme ceci:  

    void TT_AttractMode() {
         DispRow1 = DisplayUpper;
         DispRow2 = DisplayLower;
         WriteUpper("MY NEW GAME     ");}

La dernière étape de définition est le TT_SolTimes. C'est assez important car cela indique au système quel est le délai d'activation par défaut pour chaque solénoïde en millisecondes.
Il y a de nombreux types de bobines dans un flipper ainsi que des moteurs, aimant, flasheurs qui sont activé par la carte de pilotage des solénoïdes. Normalement ce temps devrait être défini avant toute activation sans ça vous ne pourrez pas les tester convenablement. Pour commencer je vous propose de mettre la valeur de 30 ms qui ne sera pas suffisant pour un moteur ou un aimant, mais au moins vous ne devriez rien cramer en raison d'un temp d'activation trop long.
Pour mon Rollergames j'ai mis 100ms pour les flasheurs, 999ms pour les moteurs et les relais. Pour les bobines 30ms est une bonne valeur pour commencer et si ça manque un peu de puissance vous pouvez essayer 50ms.
### 1.3 Ajouter un nouveau jeu
Maintenant que nous avons terminé de définir notre jeu, nous devons faire en sorte de pouvoir le choisir. Il y a une routine appelée TT_init quand TT_GameDefinition est sélectionnée.

    void TT_init() {
        ACselectRelay =  TT_ACselectRelay; // assign the number of the A/C select relay
        GameDefinition = TT_GameDefinition;} // read the game specific settings and highscores

Finalement le nouveau code peut être implémenté dans le programme APC.ino. Pour cela vous devez localiser l'entrée du menu "Active Game" dans APC_SetList.

    {" ACTIVE GAME ",HandleTextSetting,&TxTGameSelect[0][0],0,4},

Vous trouverez un paramètre TXTGameSelect qui est une liste de champs texte contenant le nom de chaque jeu implémenté. Incrémentez le dernière nombre afin d'ajouter une nouvelle entrée. Le 4 signifie qu'il y a 5 entrée (on compte à partir de 0). Ensuite ajoutez l'entrée da,s TXTGameSelect :

    char TxTGameSelect[5][17] = {{" BASE  CODE     "},{" BLACK KNIGHT   "},{"    PINBOT      "},{"  USB  CONTROL  "},{"  TUTORIAL      "}};

cela signifie que maintenant "Tutorial" sera listé dans le menu "Active Game" et la variable APC_setting[ActiveGame] vaudra 4 dans il sera sélectionné.  

La dernière étape est de localiser Init_System2 dans APC.ino ou sont appelé les routines d'initialisation de chaque jeu après que le système ait démarré. Après avoir rajouter le jeux Tutorial le "switch/case" ressemblera à ça :

    void Init_System2(byte State) {
        switch(APC_settings[ActiveGame]) { // init calls for all valid games
            case 0:
                BC_init();
                break;
            case 1:
                BK_init();
                break;
            case 2:
                PB_init();
                break;
            case 3:
                USB_init();
                break;
            case 4:
                TT_init();
                break;
            default:
                WriteUpper("NO GAMESELECTD  ");
            while (true) {}
    }

Si vous démarrez votre nouveau jeu, le flipper signalera en premier qu'il n'a pas trouvé de fichier de paramètre ou de score pour le jeu sélectionné et ensuite il affichera "MY FIRST GAME". Il est temps d'ajouter de fonctionnalités à ce jeu.

## 2 Fonction de jeu basique

Jusqu'à maintenant nous avons juste affiché du texte sur les afficheurs, il est temps d'ajouter de nouvelles fonctionnalités.

### 2.1 Les Lampes

Pour utiliser les lampes, nous devons définir le pointeur LampPattern pointer avec un tableau qui stockera l'état des lampes. Le tableau se compose de colonnes de lampes permettant de stocker l'état de 8 lampes. Cela signifie que que 8 colonnes permettrons de stocker l'était de 64 lampes. Si vous ne voulez rien de compliqué, vous n'avez pas besoin de vous en soucier, assurez vous simplement d'ajouter

    LampPattern = LampColumns; // point to the standard lamp array

a votre code. Cela permettra à votre système d'utiliser le tableau de lampes standard qui est utilisé par toutes les commandes liées aux lampes

Allumer une lampe est assez simple. Il vous suffit de connaitre le numéro de la lampe que vous souhaitez allumer à partir du manuel de votre flipper et d'utiliser

    TurnOnLamp(LampNumber);

pour l'allumer. La commande pour éteindre est

    TurnOffLamp(LampNumber);

Si vous voulez faire clignoter une lampe il y a

    AddBlinkLamp(LampNumber, Period in ms);

Pour arrêter le clignotement il faut faire

    RemoveBlinkLamp(LampNumber);

Notez que toutes les lampes ont la même fréquence de clignotement.

Avec tout ce que nous avons ajouté, votre programme devrait ressembler à ceci :

    void TT_AttractMode() {                          // Attract Mode
        DispRow1 = DisplayUpper;
        DispRow2 = DisplayLower;
        WriteUpper("MY NEW GAME     ");
        LampPattern = LampColumns;                   // point to the standard lamp array
        TurnOnLamp(53);
        AddBlinkLamp(54, 250);}

Dans cet exemple la lampe 53 est allumée en continue pendant que la lampe 54 clignote toute les 250ms (4 fois par seconde)

### 2.2 Les switchs

APC propose deux manières d'utiliser les switchs. Vous pouvez utiliser

    QuerySwitch(SwitchNumber);

pour connecter l'état du switch sélectionne. La valeur 'true' sera retournée si le switch est actif.

La deuxième méthode est d'utiliser une fonction qui sera appelée quand un switch sera activé ou relâché. Pour le moment nous souhaitons effectuer une action quand un switch est activé

    Switch_Pressed = TT_TestSW;
    Switch_Released = DummyProcess;

Si un switch est activé, la focntion TT_TestSW sera appelée avec le numéro du switch en argument. Comme nous ne souhaitons pas effectuer d'actions lorsqu'un switch est relâché, nous faisons pointer Switch_Released vers DummyProcess qui ne fait absolument rien.

### 2.3 Les Solenoides

Pour commencer, nous allons faire fonction les cibles tombantes de mon Rollergames. La fonction de gestion des switchs ressemblera à ça :

    void TT_TutorialSW(byte SwitchNo) {
        switch (SwitchNo) {
            case 8:                                       // high score reset
                digitalWrite(Blanking, LOW);                // invoke the blanking
                break;
            case 49:
            case 50:
            case 51:
                if (QuerySwitch(49)) {
                    TurnOnLamp(49);}
                if (QuerySwitch(50)) {
                    TurnOnLamp(50);}
                if (QuerySwitch(51)) {
                    TurnOnLamp(51);}
                if (QuerySwitch(49) && QuerySwitch(50) && QuerySwitch(51)) {
                    ActA_BankSol(6);
                    TurnOffLamp(49);
                    TurnOffLamp(50);
                    TurnOffLamp(51);}}}

Chaque fois qu'un switche est activé, la fonction TT_TutorialSW sera appelée avec en paramètre le numéro du switch en question. C'est un bon moyen pour centraliser l'état des switchs dans un switch/case.

Pour la présence du switch 8, il n'a rien a voir avec les cibles, il sert juste a préserver les lampes. Si vous téléversez un nouveau programme dans l'Arduino, il se peu que les afficheurs et les lampes aient une luminosité plus importante que d'habitude pendant un court instant. Cela ne devrait rien endommager, mais pour prévenir cela pressez le bouton "High Score Reset" avant d'envoyer le nouveau programme. Cela invoquera le blanking qui éteindra toutes les lampes et bobines.

La partie la plus importante dans ce code est la gestion des switchs 49, 50 et 51. Dans le RollerGames ces switchs sont situés sous les cibles tombantes et leur numéro correspond également au numéro des lampes présentes devant celles-ci. Dans mon cas je vérifie juste pour chaque cible quel switch est actif et j'allume la lampe correspondante. Si toutes les cibles sont baissées alors j'éteins les trois lampes et j'active la bobine permettant de remonter les cibles.

Si votre flipper a aussi des cibles tombantes vous n'avez qu'a changer les numéros des switchs et de la bobine et tester ce code. Attention a vos doigts car elles remontent immédiatement une fois la dernière tombée.

### 2.3 Les Timers


Maintenant, supposons que nous voulions avoir des cibles tombantes chronométrées. La plupart des machines avec des cibles chronométrées ont une lampe clignotante à proximité pour indiquer qu'il faut rapidement les toucher avant le temps imparti. Le jeu Rollergames n'en a pas, mais j'attribue simplement cette tâche à la lampe 53. Cela modifie ma fonction de gestion des switchs :

    void TT_TutorialSW(byte SwitchNo) {
static byte DropTimer = 0;
switch (SwitchNo) {
case 8:                                          // high score reset
 digitalWrite(Blanking, LOW);                   // invoke the blanking
 break;
case 49:                                         // drop targets
case 50:
case 51:
 if (QuerySwitch(49) && QuerySwitch(50) && QuerySwitch(51)) { // all targets down?
   if (DropTimer) {// timer running?
     KillTimer(DropTimer);                      // stop timer
     DropTimer = 0;                             // and indicate it
     RemoveBlinkLamp(53);}                      // turn off blinking lamp
   ActA_BankSol(6);}                            // reset drop targets
 else {                                         // not all targets down
   if (!DropTimer) {                            // timer not yet running?
     AddBlinkLamp(53, 500);                     // start blinking lamp
     DropTimer = ActivateTimer(5000, 100, TT_TutorialSW);}} // start timer for 5s
 break;
case 100:                                        // timer has run out
 DropTimer = 0;                                 // indicate it
 RemoveBlinkLamp(53);                           // turn off blinking lamp
 ActA_BankSol(6);                               // reset drop targets
 break;}}

Dans cet exemple, j'utilise un minuteur pour remonter les cibles 5 secondes après que la première soit tombée.

    DropTimer = ActivateTimer(5000, 100, TT_TutorialSW);}} // start timer for 5s

Dans cette ligne, un minuteur est activé pour 5 secondes. Après ce temps, TT_TutorialSW sera appelé avec comme argument la valeur 100 (j'ai choisi cette valeur car il n'y a pas de switch ayant ce numéro). Quand cet évènement ce produira, la variable DropTimer sera remise à zéro pour indiquer qu'il n'y a plus de minuteur ne cours ce qui signifie aussi que toutes les cibles sont en haut. Ensuite la lampe 53 est éteinte et je remonte les cibles.

Si les cibles sont atteintes dans les 5 secondes par le joueur, elles doivent être remontrée et le minuteur désactivé. Pour le désactiver il vous faut son identifiant qui est retourné par la fonction ActiveTimer à son initialisation. Dans ce cas, cet identifiant est stocké dans la variable DropTimer et la commande KillTomer(DropTimer) est utilisée pour le détruire.

Pourquoi appeller TT_TutorialSW et non une autre fonction ?

Les deux sont possible, mais en utilisant une autre fonction cela oblige à utiliser des variables globales pour stocker l'identifiant du minuteur et d'autres informations à partager entre elles. Je préfère limiter le nombre de variables globales afin que le code soit plus simple a lire.

Il n'est pas forcément nécessaire d'arrêter un minuteur du coup vous n'avez pas besoin de stocker son identifiant.  Par exemple, essayons d'éviter que les cibles tombantes soient réinitialisées immédiatement lorsque la dernière cible descend. Dans un jeu, cela pourrait faire rebondir la balle contre la vitre ou entraver la remontée des cibles car la balle se trouverait toujours au-dessus d'elles. À la place, j'aimerais attendre 200 ms avant de réinitialiser les cibles. Dans le code, vous déplaceriez la commande ActA_BankSol(6) dans une nouvelle fonction que vous nommerez TT_ResetDropTargets(byte Dummy) et vous appelleriez juste :

    ActivateTimer(200, 0, TT_ResetDropTargets);

## 3 Un vrai code de base
Jusqu'à maintenant j'ai juste expliqué comment Controller les différents éléments ce qui est nécessaire pour commencer a écrire un code de jeu complet, mais il y a encore pas mal de choses a faire avant que la première balle soit prête à être lancée. Le BaseCode contient tout le nécessaire pour ces actions élémentaires mais indispensable au bon déroulement du jeu. Mettons en place la fonction TT_AttractMode() avec les commandes nécessaire :

    void TT_AttractMode() {                               // Attract Mode
        DispRow1 = DisplayUpper;
        DispRow2 = DisplayLower;
        WriteUpper("MY NEW GAME     ");
        LampPattern = LampColumns;                        // point to the standard lamp array
        //TurnOnLamp(53);
        //AddBlinkLamp(54, 250);
        Switch_Pressed = TT_AttractModeSW;
        //Switch_Pressed = TT_TutorialSW;
        //Switch_Released = DummyProcess;}
        digitalWrite(VolumePin,HIGH);                    // set volume to zero
        LampPattern = NoLamps;
        AppByte2 = 0;
        LampReturn = TT_AttractLampCycle;
        ActivateTimer(1000, 0, TT_AttractLampCycle);
        TT_AttractDisplayCycle(0);}

La première des commandes que nous avons ajouté sert a couper le son en mettant la sortie VolumePin a l'état haut. Ensuite toutes les lampes sont éteintes en définissant le pointeur LampPattern sur NoLamps. L'étape suivante définie le pointeur LampReturn sur TT_AttractLampCycle qui contrôle les animations des lampes en utilisant la commande ShowLampPatterns pendant l'attrac mode avec un minuteur.

### 3.1 ShowLampPatterns
La commande ShowLampPatterns propose un moyen simple de créer des animations pour les lampes du plateau. Elle ignore les 8 premières lampes qui sont habituellement placées dans le fronton. Pour l'utiliser il faut créer un tableau de LampPat structures. Il est composé d'une valeur sur 16 bits qui détermine la durée pendant laquelle un motif est affiché en millisecondes, ainsi que le tableau de motifs de lampes correspondant qui utilise des octets pour définir l'état de chaque lampe. Un exemple simple se trouve au début de Tutorial.ino. TT_AttractPat1 commence avec la lampe 9 et passe à la suivante après 250 ms. Fondamentalement, cela fait la même chose que le test d'une seule lampe en mode test, mais uniquement pour les lampes à partir de 9.

    // Duration..11111110...22222111...33322222...43333333...44444444...55555554...66666555
    // Duration..65432109...43210987...21098765...09876543...87654321...65432109...43210987
           250,0b00000001,0b00000000,0b00000000,0b00000000,0b00000000,0b00000000,0b00000000},

L'exemple ci-dessus allumera la lampe 9 pendant 250 ms avant de passer à la ligne suivante. Les deux lignes ci-dessus visent à vous aider à localiser les bonnes lampes en affichant le numéro de la lampe en dessous, avec la rangée supérieure indiquant la décennie du numéro de la lampe. La dernière rangée d'un tel tableau est marquée par une durée nulle.

Pour utiliser ShowLampPattern, la variable globale PatPointer doit pointer vers le tableau de motifs et FlowRepeat spécifie le nombre de répétitions à afficher. Un programme simple pour jouer diverses séquences est présenté ci-dessous

    void TT_AttractLampCycle(byte Event) {              // play multiple lamp pattern series
        static byte Phase;
        if (Event == 1) {                               // initial call?
             Phase = 0;}                                   // reset cycle phase
        PatPointer = TT_AttractFlow[Phase].FlowPat;     // set the pointer to the current series
        FlowRepeat = TT_AttractFlow[Phase].Repeat;      // set the repetitions
        Phase++;                                        // increase counter
        if (!TT_AttractFlow[Phase].Repeat) {            // repetitions of next series = 0?
            Phase = 0;}                             // reset counter
        ShowLampPatterns(1);}                           // call the player

Voici un autre exemple d'utilisation d'un tableau TT_AttractFlow pour stocker la séquence de motifs à jouer et le nombre de répétitions. En définissant le pointeur LampReturn sur TT_AttractLampCycle, on garantit qu'il sera rappelé lorsque ShowLampPatterns aura terminé la séquence. Ensuite, il doit définir le pointeur sur la séquence suivante et exécuter à nouveau ShowLampPatterns. Afin de pouvoir arrêter les animations des lampes, vous pouvez soit ajouter une fonctionnalité d'arrêt à TT_AttractLampCycle, soit appeler ShowLampPatterns(0) en utilisant zéro comme signal pour l'arrêt.

### 3.2 Affichage pendant l'Attract mode
Habituellement pendant l'Attrac mode on affiche le nom du flipper, le score de la dernière partie et les HighScores.

    void TT_AttractDisplayCycle(byte Step) {
        TT_CheckForLockedBalls(0);
        switch (Step) {
            case 0:
                WriteUpper2("THE APC TUTORIAL");
                ActivateTimer(50, 0, ScrollUpper);
                WriteLower2("                ");
                ActivateTimer(1000, 0, ScrollLower2);
                if (NoPlayers) {                             // if there were no games before skip the next step
                    Step++;}
                else {
                    Step = 2;}
                break;
            case 1:
                WriteUpper2("                ");             // erase display
                WriteLower2("                ");
                for (i=1; i<=NoPlayers; i++) {               // display the points of all active players
                    ShowNumber(8*i-1, Points[i]);}
                ActivateTimer(50, 0, ScrollUpper);
                ActivateTimer(900, 0, ScrollLower2);
                Step++;
                break;
            case 2:                                             // Show highscores
                WriteUpper2("1>              ");
                WriteLower2("2>              ");
                for (i=0; i<3; i++) {
                    *(DisplayUpper2+8+2*i) = DispPattern1[(HallOfFame.Initials[i]-32)*2];
                    *(DisplayUpper2+8+2*i+1) = DispPattern1[(HallOfFame.Initials[i]-32)*2+1];
                    *(DisplayLower2+8+2*i) = DispPattern2[(HallOfFame.Initials[3+i]-32)*2];
                    *(DisplayLower2+8+2*i+1) = DispPattern2[(HallOfFame.Initials[3+i]-32)*2+1];}
                ShowNumber(15, HallOfFame.Scores[0]);
                ShowNumber(31, HallOfFame.Scores[1]);
                ActivateTimer(50, 0, ScrollUpper);
                ActivateTimer(900, 0, ScrollLower2);
                Step++;
                break;
            case 3:
                WriteUpper2("3>              ");
                WriteLower2("4>              ");
                for (i=0; i<3; i++) {
                    *(DisplayUpper2+8+2*i) = DispPattern1[(HallOfFame.Initials[6+i]-32)*2];
                    *(DisplayUpper2+8+2*i+1) = DispPattern1[(HallOfFame.Initials[6+i]-32)*2+1];
                    *(DisplayLower2+8+2*i) = DispPattern2[(HallOfFame.Initials[9+i]-32)*2];
                    *(DisplayLower2+8+2*i+1) = DispPattern2[(HallOfFame.Initials[9+i]-32)*2+1];}
                    ShowNumber(15, HallOfFame.Scores[2]);
                    ShowNumber(31, HallOfFame.Scores[3]);
                    ActivateTimer(50, 0, ScrollUpper);
                    ActivateTimer(900, 0, ScrollLower2);
                    Step = 0;}
                ActivateTimer(4000, Step, TT_AttractDisplayCycle);}

Pour démarrer le cycle d'affichage, la fonction PB_AttractDisplayCycle est appelée une fois depuis l'attrac mode. Ensuite elle peut être considérée comme un processus a part qui met a jour l'affichage toutes les 4 secondes.
Comme l'identifiant du minuteur n'est pas stocké, le seul moyen de l'arrêter est d'appeler la fonction KillAllTimers() qui tue tous les minuteurs en cours. Ce n'est pas la façon la plus élégante de faire, mais comme l'attrac mode comporte plusieurs processus cela va de soit de tous les tuer quand on le quitte pour lancer une partie.

L'utilisation de KillAllTimers a tout de même un gros inconvénient. La plupart de ces processus utilisent des variables statiques pour stocker l'identifiant du minuteur. Si le minuteur est tué sans que le processus en soit informé, la variable statique conservera toujours l'identifiant d'un minuteur qui n'existe plus. Si, pour une raison quelconque, on ordonne maintenant au processus de s'arrêter, il pourrait essayer de le tuer. Si vous avez de la chance, cela conduira simplement à une Erreur 11, ce qui signifie que vous avez essayé de tuer un minuteur qui n'est pas utilisé. Mais si le minuteur correspondant est maintenant utilisé par un autre processus, il sera tué, ce qui peut entraîner toutes sortes de problèmes difficiles à suivre. Pour cette raison, vous devez savoir quel processus utilise les minuteurs à quelle phase du jeu, et vous devez prendre une Erreur 11 très au sérieux lorsqu'elle se produit, car cela signifie que vous avez perdu la trace de vos minuteurs. Une Erreur 11 figera le jeu en mode débogage ; lorsque le paramètre "Mode de débogage" est réglé sur "non", l'erreur s'affichera simplement sur les affichages pendant quelques secondes, mais le jeu se poursuivra.

### 3.3 Attract mode avec gestion des switchs

Maintenant vous connaissez la procédure pour faire des animations de lampes et afficher pendant l'attrac mode, mais comment en sorti ? Pour cela nous avons besoin d'une d'une fonction de gestion des switchs. Dans la fonction TT_AttractMode() nous avons défini :

    Switch_Pressed = TT_AttractModeSW;

qui signifiqe que cette fonction sera appelée quand un switch sera activé. La fonction ressemble à ca :

    void TT_AttractModeSW(byte Button) {              // Attract Mode switch behaviour
        switch (Button) {
            case 8:                                       // high score reset
                digitalWrite(Blanking, LOW);          // invoke the blanking
                break;
            case TT_OutholeSwitch:                        // outhole
                ActivateTimer(200, 0, TT_CheckForLockedBalls); // check again in 200ms
                break;
            case 72:                                      // Service Mode
                BlinkScore(0);                        // stop score blinking
                ShowLampPatterns(0);                  // stop lamp animations
                KillAllTimers();
                BallWatchdogTimer = 0;
                CheckReleaseTimer = 0;
                LampPattern = NoLamps;                            // Turn off all lamps
                ReleaseAllSolenoids();
                if (!QuerySwitch(73)) { // Up/Down switch pressed?
                    WriteUpper("  TEST  MODE    ");
                      WriteLower("                ");
                      AppByte = 0;
                      ActivateTimer(1000, 0, TT_Testmode);}
                else {
                  Settings_Enter();}
                break;
            case 3:                                       // start game
                TT_InitGame();}}

Cette fonction ne réagi qu'a 4 switchs. Le switch 8 qui est le bouton "highscore (pour les system 7-11)" il active le blanking quand on appuie dessus ce qui arrête, afficheurs, lampes et bobines. (voir section 2.3 pour plus d'informations).

Le prochain switch est activé quand une bille arrive arrive dans le outhole, qui sera ensuite envoyée dans le chargeur.

Le switch 72 est le bouton Advance qui sert a lancer les tests ou paramétrer le flipper suivant la position du bouton Up/Down. 
Notez que les commandes BlinkScore et ShowLampPatterns sont appelées avant que la commande KillAllTimers ne soit exécutée. Cela est lié à ce que j'ai écrit dans la section précédente. Assurez vous également d'appeler ReleaseAllSolenoids après avoir utilisé KillAllTimers, car cela tuera également les minuteurs de relâchement pour les solénoïdes. Ensuite, suivant l'état du switch 73, nous lançons le mode Test ou Paramètres du jeu.

Le switch 3 est le bouton "start". Il appellera la fonction InitGame qui effectuera toutes les étapes nécessaires pour initialiser le jeu et mettre une bille dans la rampe de lancement. Il se passe ici des choses assez complexes, mais pour le moment, il vous suffit de savoir que lorsque le jeu est en cours et qu'un switch est activé, la fonction TT_GameMain est appelée. Comme vous pouvez le voir, j'ai déjà inséré la gestion de nos cibles tombantes des sections précédentes aux switchs 49, 50 et 51 (pour le jeu Rollergames, ils sont probablement différents pour votre machine). Cela signifie que vous devriez être capable de lancer une bille et de viser les cibles tombantes.

J'espère que vous avez eu un aperçu général de la manière dont fonctionne le développement de l'APC. Si vous souhaitez développer votre jeu, n'hésitez pas à poser vos questions sur l'un des forums mentionnés sur la page principale du projet. Je suis prêt à développer cette documentation en cas de besoin, mais pour le moment, la plupart des gens semblent intéressés par l'utilisation de l'APC avec PinMame et MPF, donc je me concentre sur cela pour le moment.

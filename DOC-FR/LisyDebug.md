# Utiliser Lisy

Cette page couvre simplement les bases pertinentes pour l'APC. Dans certains cas, il peut être nécessaire de lire les chapitres correspondants dans le[manuel de Lisy](https://lisy.dev/documentation.html).

Le mode de base de Lisy est sélectionné par les cavaliers Lisy_Jumpers P12, qui sont représentés ci-dessous.

![Lisy Jumper](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/LisyJumper.png)

|Jumper|Function|Comment|
|--|--|--|
|1|No Autostart|Lisy won't start PinMame after booting up|
|2|Hotspot Mode|Lisy will start a WiFi hotspot instead of using the local Wifi|
|3|Debug Mode|Writes a debug log|
|4|Lisy Control|Starts the Lisy Webserver|
|5|Not used||
|6|Not used||

## Configurer le WiFi

Toutes les fonctionnalités décrites ci-dessous nécessitent un réseau WiFi fonctionnel, ce qui signifie que vous devez installer un Raspberry Pi compatible WiFi (Zero ou Pi3) sur votre carte APC.

Pour configurer le WiFi, vous devez trouver le fichier suivant sur votre carte SD Lisy :

    /lisy/lisy/ wpa_supplicant.conf
    
et modifier les paramètres pour 'country', 'ssid' et 'psk' avec ceux de votre réseau sans fil.

Pour plus d'informations sur Lisy et le WiFi, veuillez lire la section 'Configuration sans fil' dans le manuel de Lisy.

## Mettre à jour Lisy

Avec le WiFi en fonctionnement, vous pouvez utiliser la fonction de mise à jour incrémentielle pour les mises à jour mineures au lieu de devoir écrire une nouvelle image Lisy sur la carte SD.

Avec le WiFi configuré et le système éteint, connectez le cavalier 4 (Lisy Control) et allumez votre machine.
Après le démarrage, Lisy tentera de se connecter à votre WiFi. Si la connexion réussit, l'adresse IP obtenue est affichée sur les écrans du flipper.
Saisissez cette adresse IP dans votre navigateur pour vous connecter à la page web LisyControl où vous pouvez lancer une mise à jour.

## Mode de débogage de Lisy

Lisy est livré avec un mode de débogage intégré qui enregistre un journal de débogage sur la carte SD du Raspberry Pi lorsqu'il est activé.
Avant de démarrer le mode de débogage, vous devez spécifier le type d'informations de débogage que vous souhaitez enregistrer. Pour ce faire, accédez aux 'Games Settings' du 'Remot control' et modifiez le paramètre 'Lisy Debug' en conséquence.
La valeur pour 'Lisy Debug' peut être déduite de la table suivante.

|Debug Option|Value|
|--|--|
|Displays|1|
|Switches|2|
|Lamps|4|
|Solenoids|8|
|Sound|16|

Si vous souhaitez enregistrer plus d'une option, vous devez ajouter les valeurs respectives. Par exemple, si vous souhaitez enregistrer les événements pour les interrupteurs et les solénoïdes, vous devez sélectionner 10.
N'oubliez pas de sélectionner 'Exit Settings' pour enregistrer vos modifications sur la carte SD de l'APC.

Pour démarrer le mode de débogage, éteignez votre machine et positionnez le cavalier Lisy 3 avant de la rallumer. Le système enregistrera désormais les informations de débogage sélectionnées, mais cela ne signifie pas qu'elles sont immédiatement stockées sur la carte SD de Lisy. Par conséquent, vous ne devriez pas simplement éteindre votre flipper après votre session de débogage, mais appuyer sur le bouton d'arrêt SW1, ce qui déclenchera une extinction contrôlée de Lisy. Ensuite, vous pouvez éteindre votre machine et retirer la carte SD du Raspberry Pi.

Le fichier journal se trouve dans le dossier /lisy/lisy_m/debug.

## Debug à distance

Lisy peut également être contrôlé à distance via WiFi. Dans ce mode, il affichera toutes les informations directement dans votre shell, y compris le journal de débogage. Ainsi, vous n'avez plus besoin de vous embêter à retirer la carte SD, car toutes les informations nécessaires sont déjà affichées sur votre écran.

Si vous avez configuré la configuration WiFi de votre système Lisy, vous pouvez utiliser le mode de débogage à distance comme suit :

Avec votre machine à flippers éteinte, connectez les cavaliers 1 (No Autostart) et 3 (Debug Mode).
Après avoir allumé l'alimentation, attendez que la LED jaune s'allume. Lisy a maintenant démarré et attend des instructions.
Tout d'abord, vous devez obtenir l'adresse IP de Lisy dans votre réseau WiFi. Ensuite, ouvrez un shell sur votre PC et utilisez cette adresse pour vous connecter à Lisy via ssh :
 
    ssh pi@IP-address

Connectez-vous au système en utilisant 'lisy80' comme mot de passe.
Maintenant, cela dépend que vous utilisiez le Raspberry Pi sur une carte APC 3 ou sur une carte Lisy_Mini. Dans le cas de l'APC 3, vous devez utiliser la commande suivante :
    ./run_lisy_apc
    
pour démarrer PinMame, et avec une carte Lisy_Mini, c'est

    ./run_lisy_mini
    
Ces commandes lanceront Lisy et afficheront un journal complet dans votre shell.

### Controle à distance et mises à jour

Les commandes 'run_lisy_apc' et 'run_lisy_mini' ne sont pas automatiquement mises à jour avec votre système Lisy lorsque la mise à jour est effectuée via Lisy Control. Cela signifie que vous devez modifier ces scripts manuellement.
Prenons l'exemple du script 'run_lisy_apc'. Normalement, il ressemble à ceci :

    sudo ./lisy/xpinmame.vid_lisy -nosound -skip_disclaimer -skip_gameinfo -nvram_directory /pinmame/nvram -rp /boot/lisy/lisy_m/roms lisy_apc
    #sudo /usr/local/bin/lisy -nosound -skip_disclaimer -skip_gameinfo -nvram_directory /pinmame/nvram -rp /boot/lisy/lisy_m/roms lisy_apc
    
Pour activer la mise à jour de Lisy, vous devez simplement déplacer le signe de commentaire de la deuxième à la première ligne, et c'est tout.
Notez que vous devez d'abord exécuter la commande 'rw' pour mettre Lisy en mode lecture/écriture.

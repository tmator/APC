J'ai écrit quelques scripts Perl pour effectuer certaines tâches liées à l'APC.

## Convertisseur Audio

[AudioSave.pl](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/AudioSave.pl) est un script permettant de convertir les fichiers WAV en un format directement gérable par l'Arduino. Le taux d'échantillonnage de ce fichier WAV doit être de 44,1 KHz.  
L'outil recherche un fichier WAV mono 16 bits nommé Data.wav et génère un fichier Data.bin à partir de celui-ci. Fondamentalement, il saute l'en-tête et convertit les valeurs signées sur 16 bits du fichier WAV en valeurs non signées sur 12 bits pour le DAC du DUE.
Je recommande d'utiliser des outils comme Audacity pour générer le fichier WAV mono, mais l'outil peut également être utilisé pour convertir des fichiers stéréo. Pour cela, vous devez simplement décommenter les deux lignes `read(FH, $buf1, 1);` qui font en sorte que l'outil saute chaque deuxième valeur. L'inconvénient est que cela ne générera pas un vrai fichier mono, mais éliminera simplement un canal.

Renommez Data.BIN et placez-le sur la carte SD. Utilisez uniquement des lettres majuscules pour le nom de fichier, car c'est seulement dans ce cas que l'affichage du flipper peut l'afficher correctement en cas d'affichage d'un message d'erreur.

Il existe également une version [AudioSaveFolder.pl](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/AudioSaveFolder.pl) de cet outil disponible (merci à Mokopin). Cet outil convertit automatiquement tous les fichiers .wav dans le dossier actuel en fichiers .snd.

## Calculateur de segment d'affichage

L'outil suivant génère la définition numérique des motifs de segments d'affichage (2 octets pour chaque caractère). En d'autres termes, les nombres indiquent à l'APC quels segments d'affichage activer pour chaque caractère. Dans le code, ces nombres peuvent être trouvés au début de APC.ino. Ils sont stockés dans des tableaux nommés AlphaUpper, AlphaLower et NumLower en fonction de l'endroit où vous voulez que ce caractère apparaisse.

Pour les lettres et les chiffres normaux, il ne devrait normalement pas être nécessaire de vous préoccuper de ces définitions car elles sont déjà définies. Cependant, si vous voulez des caractères spéciaux ou des effets graphiques, vous devez déterminer les numéros correspondants. Pour simplifier cela, j'ai généré trois versions de cet outil pour les différentes occasions.

* [DisplayAlphaUpper.pl](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/DisplayAlphaUpper.pl) 
* [DisplayAlphaLower.pl](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/DisplayAlphaLower.pl)
* [DisplayNumLower.pl](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/DisplayNumLower.pl)

Let's assume you have a Black Knight 2000 which has alphanumerical displays in both rows and you want to know the pattern definition numbers for the character 'A' in the upper display row. Take a look into your pinball manual or open the code of the tool to determine which segments have to be lit for an 'A'. You can see that an 'A' consists of the segments a, b, c ,e ,f, g and m. Now execute DisplayAlphaUpper.pl in a shell and type each of these segment characters followed by _Enter_. The tool will print a simple sketch of the selected segments as well as the two definition bytes. If your 'A' is complete you should end up with 126, 4 and when you look at the third line of AlphaUpper in the APC.ino you will find these values to define an 'A'.

If you want to have the values for an 'A' in the lower row you have to use DisplayAlphaLower.pl and DisplayNumLower.pl for numbers in the lower row (like in Big Guns).

Supposons que vous ayez un Black Knight 2000 qui a des afficheurs alphanumériques dans les deux rangées et que vous souhaitiez connaître les numéros de définition du motif pour le caractère 'A' dans la rangée supérieure de l'afficheur. Regardez dans le manuel de votre flipper ou ouvrez le code de l'outil pour déterminer quels segments doivent être allumés pour un 'A'. Vous pouvez voir qu'un 'A' est composé des segments a, b, c, e, f, g et m. Exécutez ensuite DisplayAlphaUpper.pl dans un terminal et tapez chacun de ces caractères de segment suivi de la touche Entrée. L'outil affichera un schéma simple des segments sélectionnés ainsi que les deux octets de définition. Si votre 'A' est complet, vous devriez obtenir 126 et 4, et lorsque vous regardez la troisième ligne d'AlphaUpper dans le fichier APC.ino, vous trouverez ces valeurs pour définir un 'A'.

Si vous souhaitez obtenir les valeurs pour un 'A' dans la rangée inférieure, vous devrez utiliser DisplayAlphaLower.pl et DisplayNumLower.pl pour les chiffres dans la rangée inférieure (comme dans Big Guns).

***
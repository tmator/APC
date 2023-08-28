# Si quelque chose ne fonctionne pas

## Sond qui sacade

Si le son saccades parfois, il s'agit très probablement d'un problème avec votre carte SD.

La première chose à essayer est de formater votre carte avec un outil de formatage de carte SD spécial, comme celui de l'Association SD (sdcard.org), qui peut améliorer le temps nécessaire à votre carte SD pour ouvrir un fichier. Cela peut être crucial, surtout pour les machines antérieures à System11, car leurs séquences sonores sont généralement composées de nombreux sons individuels. Chacun de ces sons nécessite l'ouverture d'un nouveau fichier sur la carte, et si votre carte ne peut pas ouvrir les fichiers suffisamment rapidement, les sons commencent à présenter des saccades.

Si cela ne fonctionne pas, essayez une carte SD différente. J'avais plusieurs cartes SD de la même marque, mais pour une raison quelconque, l'une de ces cartes produisait des retards sonores sur mon Pinbot, même si je l'avais formatée avec l'outil de formatage. Après avoir remplacé la carte par l'une de ses semblables, tout a fonctionné correctement.

Cependant, certaines séquences sonores peuvent tout simplement être trop exigeantes pour vos cartes SD.
La solution consiste à enregistrer toute la séquence dans un seul fichier audio et à le lire avec une priorité élevée. Cela ne nécessite l'ouverture que d'un seul fichier pour toute la séquence, et la priorité élevée bloque toutes les commandes sonores ultérieures de PinMame tant que le fichier est en cours de lecture.

## Erreur 'Unknown Command'

Si vous utilisez l'APC avec Lisy/PinMame et que le message d'erreur 'Unknown Command' suivi d'un numéro s'affiche sur vos affichages inférieurs, alors vous avez un problème avec votre communication série.

Cela se produit généralement lorsque Lisy/PinMame lit des fichiers son à partir de la carte SD. La raison en est qu'une carte SD a besoin de plusieurs millisecondes pour ouvrir un fichier. Pendant ce temps, le logiciel APC ne peut pas traiter les commandes entrantes sur l'interface série, ce qui signifie qu'elles doivent être stockées dans le tampon de réception.
La taille standard du tampon série du DUE est de 128 octets, ce qui était suffisant pour les cartes SD avec lesquelles j'ai fait mes tests. Même si elles étaient bon marché et ordinaires, il se peut que votre carte soit encore plus lente.
Bien sûr, vous pouvez essayer une autre carte SD et espérer qu'elle soit plus rapide, mais vous pouvez également augmenter la taille du tampon série. Le problème est que ce paramètre fait partie des bibliothèques Arduino, ce qui rend un peu plus compliqué à trouver, du moins avec l'IDE Arduino.

Si vous utilisez Sloeber, le plugin Arduino pour Eclipse, vous pouvez simplement regarder dans votre "Explorateur de projets" et chercher le fichier RingBuffer.h comme indiqué ci-dessous :

![Ringbuffer](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Ringbuffer.png)

Pour l'IDE Arduino, c'est un peu plus compliqué.
Dans la fenêtre de préférences de votre IDE Arduino, vous devriez voir un lien vers un fichier preferences.txt dans le coin inférieur gauche. Nous ne nous soucions pas du fichier, mais le chemin est important. Il devrait ressembler à ceci :

    C:\Users\user\AppData\Local\Arduino15 

Suivez le chemin, puis accédez à

    C:\Users\user\AppData\Local\Arduino15\packages\arduino\hardware\sam\1.6.12\cores\arduino
    
où vous devriez trouver le fichier RingBuffer.h.

Dans le fichier, il y a la ligne

    #define SERIAL_BUFFER_SIZE 128
    
qui définit la taille du tampon à 128 octets.
Définissez la taille à 256 octets et vos erreurs de 'Unknown Command' devraient disparaître.

## Clone Arduino endomagé

Si vous utilisez un clone chinois de l'Arduino DUE qui s'est soudainement arrêté de fonctionner et qui consomme un courant élevé, il s'agit très probablement d'un circuit intégré convertisseur endommagé.
Vous pouvez vérifier cela en alimentant votre DUE sur le banc (sans la carte APC) avec 5V. Ensuite, vérifiez la température du circuit intégré marqué.

![Buck](https://github.com/AmokSolderer/APC/blob/master/DOC/PICS/Buck.jpg)

Si le circuit intégré devient chaud, il est endommagé. La bonne nouvelle est que vous n'avez pas besoin de ce circuit intégré lorsque vous utilisez le DUE avec l'APC, vous pouvez donc simplement le retirer et votre système devrait fonctionner à nouveau correctement.

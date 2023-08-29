# Son avec PinMame

Pour certaines générations de flippers, vous pouvez installer la carte sond d'origine. Dans ce cas, vous ne pouvez bien sûr pas apporter de modifications liées au son. Pour les systèmes 3 à 6, la carte audio est contrôlée par des pilotes de solénoïde réservés, elle fonctionnera donc dès le départ. Le système 7 nécessite un adaptateur pour connecter la carte audio à l'interface d'extension HW de l'APC. Les systèmes 9, 11, 11a et 11b possèdent des circuits liés au son sur le CPU, donc leurs cartes son ne peuvent plus être utilisées.

La configuration actuelle n'utilise pas le Raspberry Pi pour générer les sons de PinMame. À la place, tous les fichiers son et musique doivent être créés et stockés sur la carte SD de l'APC. Bien sûr, cela nécessite un certain travail, mais nous pensons que les avantages en valent la peine.

Tout d'abord, le Pi utilise la version Unix de PinMame qui présente de gros problèmes au niveau de la qualité sonore, alors que la qualité audio de l'APC est très bonne. Mais il y a d'autres avantages à avoir tous les fichiers son sur la carte SD. L'un d'eux est que vous pouvez utiliser les mêmes fichiers pour PinMame, MPF ou lors de la programmation de l'APC en natif. Cela peut ne pas sembler très important, mais cela vous donne la possibilité de faire fonctionner votre jeu dans PinMame et d'introduire certaines modifications en utilisant [l'API de l'APC](https://github.com/AmokSolderer/APC/blob/master/DOC/Software/APC_SW_reference.pdf). Une description de cela peut être trouvée sur la page [PinaMame HowTo](https://github.com/AmokSolderer/APC/tree/master/DOC/PinMame_howto.md.

Un bon exemple est quelqu'un qui souhaite que son jeu "Disco Fever" du système 3 joue des chansons comme "Saturday Night Fever" en plus des sons normaux de la machine. Nous devons encore ajouter la prise en charge de PinMame pour le système 3, mais une fois que nous l'aurons, ajouter la musique devrait être une tâche facile. Les commandes de l'API de l'APC peuvent être utilisées pour démarrer la musique lorsque le jeu démarre et l'arrêter lorsque le jeu se termine. Vous pourriez même identifier certains points de déclenchement pour passer à une autre chanson, etc.

Pour que cela soit possible, il est nécessaire que PinMame et l'APC puissent jouer des sons simultanément, ce qui nécessiterait normalement un mélangeur audio HW supplémentaire qui mélange les deux canaux sonores ensemble. Pour éviter cela, nous avons décidé de laisser l'APC gérer complètement le traitement du son, PinMame lui indiquant simplement quel son jouer et quand.

L'inconvénient de cela est que vous devez extraire les fichiers musicaux de PinMame. De plus, nous devons comprendre comment fonctionnent les cartes audio et émuler leur comportement. La première tâche est facile et si nous trouvons un espace serveur pour stocker les fichiers quelque part sur Internet, ce travail ne doit être effectué qu'une seule fois pour chaque jeu. Pour les jeux des systèmes 3 à 9, je m'attends de toute façon à ce que cela soit facile, car ils ont des performances sonores très limitées. Vous pourriez utiliser le mode de débogage audio expliqué plus tard pour découvrir quels sons extraire.

## Available Sounds

At the moment the list of available sound packages is still quite short, but we hope that some of you will do this for their machines also. Of course it would be great if you'd share you sound package with the rest of us. Up to now we haven't found some server space to store the files on, so send me a PM via Pinside or Flippertreff (see the feedback section on the main page) if you want to have one of the sound packages listed below.

|System|Game| Sound File URL|Comments|
|--|--|--|--|
|6|Firepower| Ask Matiou in the Pinside forum | PinMameExceptions also done by Matiou |
|7|Barracora| URL available on request| |
|7|Black Knight| URL available on request| |
|7|Jungle Lord| URL available on request| Works great. Sound samples are also good. |
|7|Pharaoh| Ask Grangeomatic in the Pinside forum| PinMameExceptions also done by Grangeomatic |
|9|Comet|URL available on request| Works great. Sound samples are also good. |
|11a|Pinbot| URL available on request| Some PinMame sounds are quite bad, e.g. visor opening and closing. May be someone can sample them from the orignal HW.|
|11a|F-14 Tomcat| Ask Snux in the Pinside forum | PinMameExceptions also done by Snux |
|11c|Rollergames| URL available on request| There're problem with this game. For some reason PinMame restarts random music tracks all 5 seconds. The issue doesn't seem to affect the Windows version of PinMame. Please contact me if you have any idea what the root cause might be. |

## Cartes son

Comprendre les cartes son est la deuxième tâche à accomplir. Encore une fois, je m'attends à ce que ce soit facile pour les jeux antérieurs au Système 11, mais cela m'a pris environ une semaine avant d'être satisfait des sons de mon Pinbot. Bien sûr, j'ai commencé de zéro, donc ce sera plus facile pour la personne suivante qui fera cela, mais le Système 11 comporte plusieurs cartes audio et je ne sais pas dans quelle mesure elles diffèrent de celles du Pinbot. Pour cette raison, je vais maintenir une deuxième liste ici où toutes les informations pertinentes de PinMame concernant les différents systèmes sont répertoriées.
Hélas, les commandes sonores semblent être uniques pour chaque machine, ce qui nécessite une gestion légèrement différente des commandes sonores en fonction de la machine sélectionnée. Le logiciel APC comporte une gestion des exceptions spécifiques à la machine qui vous permet de mettre en œuvre ces commandes audio spéciales pour votre machine. Une description de ces fonctions peut être trouvée sur la [page PinMame HowTo](https://github.com/AmokSolderer/APC/tree/master/DOC/PinMame_howto.md).

### System 7

Jusqu'à présent, nous connaissons des commandes standard qui semblent être identiques pour les cartes audio du système 7 :

|Command / Hex|Comment|
|--|--|
|2c|Arrêter le son|
|7f|Pas une vraie commande sonore, mais utilisée pour réinitialiser le bus de données vers la carte audio entre les commandes|

Ces cartes comportent ce que j'appelle une série sonore. Cela signifie que si un certain numéro de son est appelé plusieurs fois, une version différente de ce son est jouée (généralement avec une hauteur de tonalité plus élevée). Pour l'APC, les noms de ces fichiers son doivent ressembler à 0_2a_001, où le zéro initial sélectionne le canal sonore 0 (les jeux du système 7 n'utilisent que ce canal), 2a étant le numéro du son et 001 la tonalité de ce son. Le dernier numéro est compté pour toutes les tonalités disponibles pour ce son.
L'une de ces séries sonores est généralement le son de fond (BG sound). Ce son peut être interrompu par d'autres sons mais continue ensuite. Comme les numéros de ces sons diffèrent d'un jeu à l'autre, ils doivent être définis comme une exception pour chaque jeu.

### System 11

Les premières machines System 11 avaient deux circuits audio indépendants, chacun contenant un processeur et des EPROM sonores. L'un de ces circuits était situé sur la carte CPU principale, tandis que l'autre était sur une carte audio distincte.
Dans le cas du Pinbot, PinMame distingue ces cartes en utilisant deux préfixes audio qui peuvent être sélectionnés par 00 et 01 dans le 'Sound Command Mode' de PinMame. Les deux de ces préfixes offrent différentes commandes sonores. Notre état actuel de ce que nous savons sur ces commandes sonores est répertorié ci-dessous. Il y a encore beaucoup de lacunes, mais nous ne voulions pas investir trop d'efforts dans l'étude de ces cartes. Encore une fois, nous considérons cela davantage comme une tâche communautaire et il y a probablement beaucoup de personnes là-bas ayant beaucoup plus de connaissances sur ce sujet que nous.

Donc, si vous avez des informations supplémentaires pour combler ces lacunes, veuillez nous le faire savoir et nous les inclurons.

|Prefix|Command / Hex|Comment|
|--|--|--|
|00|00|	Arrêter le son|
|01|00|	Arrêter la musique|
|01|01 - 08|Pistes de musique en boucle, certaines d'entre elles ont une introduction avant que la partie en boucle ne commence|
|01|40 - 42|Transitions et fins pour la musique en boucle|
|01|7f|Arrêter les sons du préfixe 01|
|01|6X|Réglage du volume de la musique, 0x60 est le volume maximal, 0x64 est presque rien|

Le préfixe 01 joue de la musique et des sons simultanément. Il semble que les numéros de son en dessous de 0x80 soient destinés à la musique et le reste aux sons. Comme l'APC ne dispose que de deux canaux sonores indépendants, seule la musique du préfixe 01 est jouée sur le canal musical de l'APC, tandis que les sons sont transmis au canal sonore.

A partir de certains système 11B, tout le circuit audio a été retiré de la carte CPU et la fonctionnalité a été ajoutée à la carte son. Pour ces jeux, seul le préfixe 01 est utilisé par PinMame.

## Comment le faire fonctionner

C'est tout pour la théorie. Passons à la [pratique](https://github.com/AmokSolderer/APC/tree/master/DOC/PinMame_howto.md).

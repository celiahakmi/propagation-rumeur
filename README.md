# propagation-rumeur

Propagation d'une rumeur par Iván, Karene et Célia

Il peut être intéressant d'étudier la propagation d'une rumeur dans le temps au sein d'un lycée parisien, en jouant avec différents paramètres sociaux comme le niveau d'amitié ou d'influence des élèves. Cela nous permet de mieux comprendre le phénomène de diffusion d'informations dans une structure fermée

Notre modèle se base sur cette question principale : Comment différents paramètres sociaux (amitiés, nombre de contacts, rôle des élèves, influence) influencent-ils la vitesse et l'étendue de propagation d'une rumeur dans un lycée ?

Notre modèle est fortement inspiré du modèle SIR classique en épidémiologie, et plus précisément du modèle Daley-Kendall (qu'on développera un peu plus dans la suite) qui décrit la propagation des rumeurs. Nous avons adapté ce cadre au contexte d’un lycée parisien.Nous avons adapté ce cadre au contexte d'un lycée parisien.

Nous avons conçu ce modèle en trois parties, où chacune repose sur une logique similaire mais avec des différences d'implémentation, afin d'obtenir une vision plus riche et complémentaire du phénomène.

Pour nos 3 modèles, on considère un lycée de 475 élèves. Ce nombre est basé sur les statistiques de l’Observatoire des Territoires [1].

On considère quatre types d'individus:

Le Créateur (C) : Cet individu est la personne qui crée la rumeur. C’est le premier propagateur. Il est toujours placé à l’indice 0 de la liste lycée.
Les Ignorants (I) : Ce sont les individus qui ne connaissent pas la rumeur. Ils ne peuvent évidemment pas la propager. Au début, toute la population est ignorante, sauf le Créateur.
Les Propagateurs (P) : Ce sont ceux qui connaissent la rumeur et la propagent à x personnes (défini par un paramètre aléatoire).
Les Oubliants (O) : Ce sont ceux qui connaissent la rumeur, mais ne la propagent plus.
La distinction Ignorants - Propagateurs - Oubliants est directement inspirée du modèle de Daley-Kendall (1965) sur la propagation des rumeurs [2].

Enfin, nous étudierons ce système dynamique sur une periode de 30 jours, soit 1 mois. On considère donc max_tours=30.

Notre modèle se constitue en trois parties :

Première partie: Modèle Simple

Ce modèle simple constitue une parfaite introduction à notre système. Il permet d'avoir une approche plus intuitive avant d'intégrer des paramètres plus complexes. Nous nous inspirons du modèle linéaire de Schelling vu en cours [3] et du modèle SIR [4]. Toutefois, ici, les transitions ne sont pas déterminées par des formules mais par des processus aléatoires.

Dans cette première partie, nous avons 6 fonctions:

world_lycee(lycee,size): qui prend en paramètre la liste lycée, soit notre monde et size, soit la taille désiré de notre monde. Cette fonction crée une liste avec size-1 Ignorants et 1 Createur qui est placer à l'indice 0.
get_propagateurs(lycee): Cette fonction prend en paramètre lycée et renvoie une liste avec l'indice des personnes propagateurs. Au début, la fonction renvoie une liste avec le chiffre 0.
get_ignorants(lycee): Cette fonction prend en paramètre lycée et renvoie une liste avec l'indice des personnes ignorantes. Au début, la fonction renvoie une liste avec les chiffres compris entre 1 et size -1.
propagation(lycee): Cette fonction prend en paramètre lycée et renvoie une liste avec la propagation faite. Chaque propagateur propage la rumeur à n personnes, soit _ personnes_rencontrees_ sur notre fonction. _ personnes_rencontrees_ est un chiffre aléatoire compris entre 0 et 30. Donc un propagateur peut propager la rumeur à 30 personnes maximum. Cela se justifie car, approximativement, une classe d'élèves dans un lycée en France est composée de 30 élèves.(5).
nb_jours(lycee): Cette fonction fait la simulation complète du modèle simple. Elle renvoie le nombre de jours nécessaires pour que tout les lycéens de ce lycée connaissent la rumeur.

<img width="723" height="570" alt="Capture d’écran 2026-05-05 à 19 49 40" src="https://github.com/user-attachments/assets/71e9469e-0a20-4bd8-b9df-bc9d7d5ed7e8" />

La fonction nous donne ce graphique. On peut voir l'évolution des ignorants et les propagateurs par rapport au jours. Au bout de 2 jours, toute la population du lycée est connaisseur de la rumeur.

Deuxième partie: Modèle avec les niveaux d'amitié.

Dans cette deuxième partie on introduit un nouveau paramètre: Les niveaux d'amitiés. En effet, la propagation était rapide dans la première partie pusisqu'on ne considère pas si deux personnes sont amies ou pas. Avec ces nouvelles implémentations, nous pouvons régler ce problème. \

L'idée est la suivante: Une personne qui est propagatrice peut propager la rumeur seulement si la somme du niveau d'amitié de cette personne avec le niveau d'amitié de la personne sélectionné est supérieur ou égale à seuil. Par exemple: Une personne a un niveau d'amitié de 0.5 et l'autre personne a un niveau d'amitié de 0.3. On définit seuil en 0.9. La somme de ces deux niveaux d'amitiés est égale à 0.8, ce qui est inférieur à seuil. Alors, la rumeur ne se propage pas.

Pour faire possible cet algorithme, on a crée les fonctions suivantes:

lycee_avec_amis(size) : Cette fonction créé une liste qui a la même taille que lycee. Cette liste contient des chiffres aléatoires compris entre 0 et 0.9. L'élement à lycee_niv_ami[i] est le niveau d'amitié de l'individu lycee[i].
propagation_avec_ami(lycee_ami, lycee_niv_ami, seuil): Cette fonction fonctionne du même principe que la fonction propagation du modèle simple sauf qu'ici, si la personne, qui est choisit au hasard, et le propagateur n'ont pas un niveau suffisant d'amitié, la rumeur ne se propage pas.
nb_jours_ami(lycee_ami,lycee_niv_ami,seuil): Cette fonction fait la simulation complète. Elle calcule combien de jours la rumeur va prendre pour être connue par tout les élèves du lycée.\
Comme nous choisissons le nombre de personnes_rencontrees au hasard, le résultat change. Donc, nous avons besoin de créer un graphique qui nous permettra de voir si, effectivement, l'ajout de cette nouvelle contrainte a un effet sur notre système dynamique.

<img width="742" height="472" alt="Capture d’écran 2026-05-11 à 22 25 14" src="https://github.com/user-attachments/assets/0a8f9aa8-7053-4b2c-aeca-cbe96e15446a" />

On aperçoit que, lorsqu'on ajoute la condition de l'amitié, les jours que la rumeur prend pour être entièrement propager double.\

Maintenant intéressons-nous au paramètre seuil. Ça peut être intéressant de jouer avec la valeur de seuil et d'identifier lorsque la rumeur dépasse le nombre de tours maximal, soit 2 mois. Pour observer ça, nous avons crée une fonction differents_seuil(lycee_n,lycee_niv_ami_n)qui fait la fonction nb_jours_amiavec differents seuils entre 0 et 0.9.

<img width="689" height="551" alt="Capture d’écran 2026-05-11 à 22 25 42" src="https://github.com/user-attachments/assets/69fd9791-6273-4c01-a014-f8bc1152afe5" />

Dans ce graphique on peut voir que lorsque le seuil est supérieur à 1, la rumeur se propage très lentement. Avec un seuil d'amitié élevé, on doit avoir beacoup plus de confiance pour transmettre la rumeur à une personne.Il n'y a pas une bonne relation entre les élèves du lycée. Cela explique pourquoi la rumeur mets beaucoup de temps à se propager.

Intéressons-nous maintenant à la variabilité de la propagation.

simulations_propagation(nb_simulations, size, seuil): Cette fonction permet d'observer la variabilité de la propagation total d'une rumeur sur plusieurs simulations effectués. Elle reprend la fonction ... du modèle de Schelling (3). Grâce à cette fonction, nous pouvons comparer et observer la propagation total sur différentes simulations effectués.
Depuis cette fonction, nous pouvons en créer un graphique afin de visualier ces différentes simulations de manière plus représentative et claire. Cela permet de constater que durant chaque simulation( au total 3), la rumeur se propage sur un nombre de jours différents (= ça prouve pas l'influence et l'intérets des paramètres)

<img width="556" height="445" alt="Capture d’écran 2026-05-11 à 22 26 09" src="https://github.com/user-attachments/assets/fcadbb35-a916-4279-b58a-c350f6c5cba0" />

Nous pouvons en conclure que la propagation n'est pas toujours rapide ni constantes et que des facteurs (ex: niveaux d'amitiés) y jouent un rôle important. Bien évidemment, nous pouvons augmenter le nombre de simulations pour avoir une tendance beaucoup plus générale.

contacts_necessaires(size, seuil, essais): La fonction permet de connaître le nombre de contacts (pas forcément de l'amitié, mais simplement une rencontre, une discussion, un contact peut importe qu'il soit) qu'il faut en moyenne pour qu'un élève ignorant devienne propagateur et, donc, que la rumeur continue de se propager. À chaque tour de simulation, on compte le nombre de tentatives de contact faites par les propagateurs que l'on compte, ensuite, en une moyenne sur plusieurs simulations.
Nous pouvons en crée un graphique du nombre moyen de contacts nécessaires selon le seuil.

<img width="566" height="451" alt="Capture d’écran 2026-05-11 à 22 26 36" src="https://github.com/user-attachments/assets/edd8845f-5bcd-4ee1-a9d5-784bd6b0a04e" />

Ce graphique montre l'impact du seuil d'amitié sur la difficulté à propager une rumeur. Il n'y a d'évolution strictement croissante. Ces variations régulières s'expliquent par la dimension aléatoire du modèle et du nombre limité de simulations (essais=3). La différence reste tout de même assez stable.

Troisième partie: Modèle avec oubliants et niveaux d’amitié

Dans cette troisième partie , on ajoute une nouvelle contrainte réaliste : certaines personnes peuvent oublier la rumeur et donc ne plus la propager
L’idée est la suivante: un propagateur peut soit transmettre la rumeur à un ignorant si le niveau d’amitié total entre eux dépasse un certain seuil,soit oublier la rumeur avec une probabilité fixée (prob_oubli), et devient alors un oubliant (O).
les fonctions\

propagation_avec_ami_et_oubliants(lycee, lycee_ami, seuil, prob_oubli): cette fonction fonctionne comme la propagation précédente, mais ici chaque propagateur peut aussi oublier la rumeur avec une certaine probabilité. Si la somme des niveaux d’amitié entre le propagateur et la personne rencontrée dépasse le seuil, la rumeur se propage, sinon elle ne se propage pas.
nb_jours_ami_et_oubliants(seuil, prob_oubli, max_tours): cette fonction fait la simulation complète de la propagation avec seuil et oubli. Elle calcule combien de jours la rumeur va prendre pour être apprise pour tout le lycée

<img width="724" height="443" alt="Capture d’écran 2026-05-11 à 22 27 01" src="https://github.com/user-attachments/assets/47a0055f-3f97-41c4-a0ae-7c51ec29df60" />

Dans ce graphique on peut voir que le nombre d’oubliants augmente lentement au début, puis plus rapidement vers la fin. Cela montre que certains propagateurs oublient la rumeur avec le temps.
Le nombre de propagateurs augmente aussi, mais pas de façon explosive, car une partie d’entre eux deviennent oubliants au lieu de continuer la propagation.


Notre projet a permis de modéliser et d'explorer la propagation d'une rumeur dans un lycée en utilisant des outils simples mais puissants inspirés des modèles SIR et Daley-Kendall. Nous avons montré que la structure sociale (niveau d'amitié, nombre de contacts, oubli) influence la vitesse et l'ampleur de la propagation.

Cependant, ce modèle pourrait, pour aller plus loin, prendre en compte d'autres paramètres importants de la vie scolaire. Voici les extensions possibles :

segmenter le lycée en classes
intégrer des niveaux d'influence différents entre les élèves: certains élèves peuvent être considérés comme populaires par exemple
considérer la temporalité des contacts: certaines interactions peuvent se faire plus fréquemment
En enrichissant notre modèle, on pourrait l'associer davantage à des observations réelles d'un lycée.

Bibliographie

[1]:( https://www.observatoire-des-territoires.gouv.fr/nombre-moyen-deleves-par-lycee-general-technologique-etou-professionnel).
[2]: Daley, D.J., & Kendall, D.G. (1965). "Stochastic rumours." Journal of the Institute of Mathematics and its Applications, 1(1), 42–55.
[3]: Thomas C. Schelling, Dynamic Models of Segregation , Journal of Mathematical Sociology (1971) 143-186
[4]: (https://mathworld.wolfram.com/Kermack-McKendrickModel.html) .
[5]: (https://www.education.gouv.fr/les-chiffres-cles-du-systeme-educatif-6515) .





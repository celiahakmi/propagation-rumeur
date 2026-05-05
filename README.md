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

# Réflexions TD4 - Séries temporelles

## 1. Comment vérifier que des données réelles existaient
Il aurait suffi de lire la documentation de l'API Open-Meteo Air Quality, qui indique le paramètre past_days, ou de faire un seul appel test avec past_days=90 dans le navigateur et de compter les valeurs reçues. C'est quelques minutes de vérification, alors que l'hypothèse fausse a fait construire tout un exercice sur des données inventées. Une hypothèse sur les données disponibles doit être vérifiée avant de concevoir quoi que ce soit autour.
Résultat : la pente est de +0,160 ppm par jour, soit environ +14 ppm sur les 90 jours. Ma prédiction était donc juste. Les valeurs récentes sont bien plus hautes que celles de l'été, ce qui correspond à la fin de la saison.
## 2. Ma prédiction pour le CO2 au Mans (avant de lancer)
Je pense que le CO2 est plutôt en hausse sur les 3 derniers mois. En été, les plantes absorbent beaucoup de CO2, puis en automne elles perdent leurs feuilles et absorbent beaucoup moins, et en plus le chauffage redémarre. Le minimum devrait donc être vers fin septembre, puis remonter.

## 3. Zéro trou détecté : faut-il supprimer la détection des trous ?
Non. Si mon exécution n'a aucun trou, ça prouve seulement que la source était complète cette fois-ci, pas qu'elle le sera toujours. L'API peut changer ou tomber en panne, et on pourrait un jour brancher un vrai capteur qui perd des mesures. Sans cette détection, un jour manquant passerait inaperçu et fausserait la moyenne mobile et la tendance. Ce code ne coûte presque rien et protège contre ce cas.

## 4. Moyenne mobile vs lissage exponentiel
Mes 5 dernières valeurs brutes sont 449,6 / 459,5 / 470,6 / 463,6 / 454,3 ppm, avec un pic à 470,6. Au jour du pic, le lissage exponentiel est monté à 457,4 alors que la moyenne mobile n'était qu'à 453,5 ; et maintenant que le CO2 redescend, le lissage exponentiel baisse déjà alors que la moyenne mobile continue de monter.

## 5. Que se passe-t-il si l'appel à l'API échoue ?
Si les 4 tentatives échouent, fetch_series.py s'arrête avec une erreur avant d'écrire series.csv, donc le workflow s'arrête aussi : analyze_series.py et le commit ne sont jamais lancés. Les deux fichiers gardent donc les valeurs de la semaine précédente. Le dashboard ne casse pas, il continue d'afficher les anciennes données, mais elles deviennent périmées sans alerte visible. Le seul indice est la date « Data last analyzed » en bas de la page, et la croix rouge dans l'onglet Actions.

## 6. Jusqu'à combien de jours je fais confiance à la projection

Ma pente est de +0,160 ppm par jour, donc la projection ne monte que d'environ 1 ppm sur 7 jours (de 449,8 à 450,8). Mais dans les vraies données, le CO2 varie de plus de 10 ppm d'un jour à l'autre (de 459,5 à 470,6 en un jour), à cause de la météo : vent, air stagnant, chauffage. En plus, la projection commence à 449,8 alors que la dernière valeur réelle est 454,3, donc elle est déjà décalée dès le premier jour.

Je ne ferais confiance à cette projection que 2 ou 3 jours, et seulement pour dire « le CO2 va rester autour de 450-460 ppm », pas pour la valeur exacte. Au-delà, la météo pèse beaucoup plus que la tendance, et une droite ne peut pas prévoir le changement de saison.
## 7. Pourquoi le pipeline marche encore après mon changement
J'ai remplacé Le Mans par Marseille en changeant une seule ligne (LAT, LON dans fetch_series.py), plus le nom de la ville écrit en dur dans index.html. Tout le reste a marché sans modification, parce que les fonctions de analyze_series.py ne mentionnent jamais le lieu ni le CO2, elles travaillent seulement sur une liste de dates et de « value ». Elles ne savent pas ce que représentent les chiffres, donc elles fonctionnent pour n'importe quelle ville. 

Le résultat est très différent : au Mans la pente était de +0,160 ppm par jour (environ +14 ppm sur 90 jours), alors qu'à Marseille elle est quasi nulle (-0,010 ppm par jour). Je pense que l'été de ce coté, très sec, fait que la végétation absorbe peu de CO2 en été, donc il n'y a pas de creux estival suivi d'une remontée comme au Mans. Les émissions de la ville et de la zone portuaire maintiennent aussi un niveau élevé toute l'année. Ça montre qu'une tendance mesurée à un endroit ne se généralise pas à un autre, même dans le même pays.

# Réflexions TD4 - Séries temporelles

## 1. Comment vérifier que des données réelles existaient
Il aurait suffi de lire la documentation de l'API Open-Meteo Air Quality, qui indique le paramètre past_days, ou de faire un seul appel test avec past_days=90 dans le navigateur et de compter les valeurs reçues. C'est quelques minutes de vérification, alors que l'hypothèse fausse a fait construire tout un exercice sur des données inventées. Une hypothèse sur les données disponibles doit être vérifiée avant de concevoir quoi que ce soit autour.

## 2. Ma prédiction pour le CO2 au Mans (avant de lancer)
Je pense que le CO2 est plutôt en hausse sur les 90 derniers jours. En été, les plantes absorbent beaucoup de CO2, puis en automne elles perdent leurs feuilles et absorbent beaucoup moins, et en plus le chauffage redémarre. Le minimum devrait donc être vers fin septembre, puis remonter.

## 3. Zéro trou détecté : faut-il supprimer la détection des trous ?
Non. Si mon exécution n'a aucun trou, ça prouve seulement que la source était complète cette fois-ci, pas qu'elle le sera toujours. L'API peut changer ou tomber en panne, et on pourrait un jour brancher un vrai capteur qui perd des mesures. Sans cette détection, un jour manquant passerait inaperçu et fausserait la moyenne mobile et la tendance. Ce code ne coûte presque rien et protège contre ce cas.

## 4. Moyenne mobile vs lissage exponentiel

## 5. Que se passe-t-il si l'appel à l'API échoue ?

## 6. Jusqu'à combien de jours je fais confiance à la projection

## 7. Pourquoi le pipeline marche encore après mon changement

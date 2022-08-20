# recap of versions

$$Version initiale v0.0.1 :$$ 
commit 0e55d70092a20d06722d17e9126cdaa5fb12ce68
Author: Ibrahim <Ibrahim75madi@gmail.com>
Date:   Sun Aug 14 13:18:09 2022 +0200

Ceci est la première version du projet, à ce stade les objectifs fixés sont les suivants :
- Créer un site qui permette de consulter facilement le prix actuel des cryptomonnaies,
- On doit pouvoir en rechercher via une barre de recherche, ou encore trier alphabétiquement les résultats de nos recherches,
- On doit pouvoir disposer des informations en temps réel

Une fois ces objectifs réalisés ils auront vocation à évoluer.

Dans cette version retrouve :
    - l'application Vue Js par défaut,
    - La liaison à l'api via Axios,
    - L'affichage des données suivantes :
      - symbol,
      - lastPrice,
      - weightedAvgPrice,
- les fonctionnalités suivantes : 
  - via la méthode UpdatePost() -> permet d'effectuer une requête pour mettre à jour les données reçu par l'API,
  - via la méthode Sort() et SortRev() qui permettent respectivement de trier les données dans l'ordre alphabétique de leur propriété symbol (par nom autrement dit),
  - On y retrouve la méthode tri(Trieur) -> permet de réaliser une recherche via un input de type text, l'insertion est passé en paramètre sous le nom de trieur,
  - la méthode Arrondir() qui permet d'arrondir ou non les valeurs affichées et les 'symbol' des éléments à l'aide d'un '...'.

$$ajout d'un toUpperCase() sur la methode Tri v0.0.11$$
commit ddc8348ce712b9172b3f5217418a237f1392c430 (master)
Author: Ibrahim <Ibrahim75madi@gmail.com>
Date:   Sun Aug 14 13:21:54 2022 +0200

Dans cette version, la fonctionnalités de recherche est améliorée. Il n'est plus nécessaire de réaliser la recherche en majuscule grâce à l'ajout d'un .toUpperCase() sur la méthode Tri().

$$arrondir() => SetModePro() v0.0.111$$
commit 4c4b59f54816c38580d76a4651d7a114b651f44e (HEAD -> modePro-experimental)
Author: Ibrahim <Ibrahim75madi@gmail.com>
Date:   Sun Aug 14 15:34:54 2022 +0200

Dans cette version la fonction arrondir() devient SetModePro() et aura à l'avenir un usage beaucoup plus large dans la perspective de permettre une vision plus détaillées des informations via un bouton on/off 'Mode Pro'

$$Amélioration code et affichage v0.0.12$$

Liste des améliorations :
- modification des méthodes de manière conventionnelle,
- ajout d'affichage pour le mode Pro, 
- v-if pour afficher le nom de la devise pour chaque élément, 
- ajout de style pour le mode Pro

$$v0.0.121$$
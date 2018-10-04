# Résumé:

Mon travail a consisté à rechercher des solutions à l'analyse des décisions de justice au niveau granulaire des demandes des parties. En effet, l'objectif est de pouvoir mettre à la disposition de potentiels utilisateurs un outil simple d'exploration, de recherche d'information et d'analyse au moins descriptive et à la longue prédictive du sens des réponses des juges aux différentes demandes dans un corpus bien identifié. Les documents de décisions judiciaires n'étant pas structurés, les problématiques de recherche tiens donc en l'extraction et la catégorisation des données nécessaires à ces différents usages. Pour savoir quelles données sont nécessaires il faut partir des fonctionnalités qu'on souhaite réaliser ou plus précisément des cas d'utilisation du système à réaliser. Il s'agit plus précisement de cas d'analyses descriptive et en voici quelques unes:

*  la recherche des décisions en fonction de critère comme : le lieu, les lois, le type de demande, la période, les circonstances factuelles ...
*  la visualisation comparative du nombre de demandes acceptées et de demandes rejettées
*  la visualisation comparative des quanta accordés et des quanta demandés
*  la visualisation comparative de l'acceptation des demandes dans l'espace 
*  la visualisation  de l'évolution du taux de demandes acceptées et de quanta accordés dans le temps

Ainsi, les données nécessaires à ces différents cas d'utilisation sont:
*  les références de la décision: lieu (juridiction, ville), la date de prononcée, les normes utilisées
*  les données sur les demandes: catégorie de demande, quantum demandé, sens du résultat, quantum accordé
*  les circonstances factuelles: une catégorisation des faits qui définit les différentes situations dans lesquelles une catégorie de demande peut être émise.

L'extraction des références a été formulée comme de détection d'entité par étiquetage de texte. La raison étant que ces métadonnées sont directement accessibles à partir du texte. Leurs positions  peuvent être facilement apprises par un modèle probabiliste graphique. Nous avons ainsi expérimenté le CRF qui est le modèle de référence en matière de reconnaissance d'entités nommées.

Pour extraire les demandes, nous avons établi une analogie avec la problématique d'extraction d'évènements. C'est ainsi que nous pouvons expérimenter des approches de même type que ceux proposés pour l'extraction des évènements. Nous avons expérimenté par exemple une approche à base de règle, une chaine de classifieur de candidats, et un modèle de prédiction structurée. Les catégories étant prédéfinies, il est possible d'annotées des données pour faire des expérimentations et même superviser l'entrainement des modèles. 

Les circonstances factuelles ne sont pas décrites dans une section particulière du document. Elles ne sont non plus décrites conjointement avec les demandes (toutes les catégories de demandes se partagent les circonstances factuelles présente dans le document). Les faits traités en justice couvrent un domaine infini car il s'agit de catégoriser les faits courants de la vie. Ainsi il serait déraisonable de penser à un apprentissage supervisé de ces situations factuelles. Deux voies s'ouvrent ainsi à nous: 

*  la modélisation de thématique consiste à réaliser des combinaisons des termes des documents d'une catégorie de demande. Chaque combinaison est associable à une circonstance factuelle. L'avantage de cette voie est de pouvoir découvrir plusieurs circonstances factuelles dans un même document. La difficultés revient à distinguer les thématiques qui sont des circonstances factuelles de celle qui n'en sont pas. Dans un document il n'y a en effet pas que les faits mais aussi des demandes de catégories différentes et surtout des termes pas pertinents pour cette tâche.

*  Le clustering de documents consiste à regrouper de manière non supervisée les documents d'un corpus selon la similarité de leur contenu. Il est question dans notre cas de modéliser la similarité de telle sorte qu'elle soit associée aux faits. Nous pouvons supposer que si toutes décisions sont prises dans la même catégorie, elles ne peuvent se distinguer que dans des groupes associables aux faits. Cette hypothèse est risquée car d'autres catégories sont présentes dans la décision. Il serait peut-être nécessaire d'elliminer les termes caractéristiques des catégories de demandes ou les passages d'expression de ces demande.Il n'en restera alors que les termes relatifs aux faits. Plusieurs autres problématiques se pose ici : le choix de l'algorithme de clustering, quelle représentation pour les textes et surtout comment mésurer la (dis) similarité entre les textes. Outre les approches classiques comme le kmeans + distance euclidienne + TF.IDF, nous proposons d'une part un apprentissage forcé de la distance entre texte + kmeans, et d'autre part une combinaison k-medoids + Word Mover's Distance (WMD). l'évaluation est supervisée sur un corpus annotées et non supervisé pour d'autre corpus.


# Structucture du mémoire

## résumé

## Introduction
L'introduction présente le contexte, les motivations et les objectifs du projet. Nous y décrivons ensuite brièvement les problématiques techniques soulevées tout en les justifiant. Ensuite nous discutons de la méthodologie de travail. Les résultats obtenus mettent en avant les contributions de la thèse en terme d'études empiriques et comparatives, ainsi qu'en terme de proposition de modèles plus ou moins propres au projet.

## la synthèse litéraire ou contexte litéraire de l'étude ou travaux connexes
Elle brosse l'ensemble des travaux similaires au projet sur les plans du domaines, des problématiques et de l'objectif. 
les problématiques étant similaires aux problématiques générales traités en analyse textuelle, l'accent dans cette section est mise sur la présentation du principes des états de l'art, et des résultats déjà obtenus sur les décisions de justices.

## les 3 chapitres de recherche empirique (évaluation intrinsèque)
Ces chapitres décrivent avec précision et illustrations, les problématiques et hypothèses traitées dans le chapitres. 
*  structuration et reconnaissance d'information d'indexation des documents
*  extraction des demandes: expérimentation distinguant les documents à une et un nombre varié de demandes
*  catégorisation des documents par circonstances factuelles: évaluations supervisées et non supervisées

## le chapitre de démonstration (évaluation extrinsèque)
Ce chapitre décrit l'utilisabilité des approches proposées et illustre leur usage sur une quantité de données approchant une échelle réelle. L'illustration tourne autour de l'analyse descriptive de la masse de documents que nous avons rassemblés. Des mises à l'échelle sont indiquées sur le niveau d'erreur présent dans la base de données. Comme dans les conférence le chapitre doit tenir sur 4/5 pages au maximum. 

## conclusion
*  Résumé des contributions avec précision sur leur atouts et leur limite. 
*  Proposition d'évolution des travaux de recherche
*  Discussion sur les conséquences d'un tel outil sur le futur du domaine d'application


# Rédaction à faire


# Manip A faire 

*  voir s'il est possible de tout remettre en python pour la démo (les modèles deep étant en python peut-être pas toucher au code en JAVA de modélisation de séquences)

## Structuration et NER
*  plus de features apprises: topic, LSA, word clustering, ngram
*  intégrer l'affinement du sectionnement à 4 / 5 sections
*  justifier et comparer l'impact des éléments atomiques : ligne / mots (pour le sectionnement)
*  comparer différents schémas d'étiquetage pour les modèles deep learning
*  étudier l'impact du nombre d'exemples d'apprentissage pour les modèles deep learning
*  proposer un enchainement logique à la sélections des différents aspects de modélisation
*  si possible proposer un modèle joint de sectionnement et de NER (en considérant une phrase comme étant une ligne d'un doc == étiquetage au niveau des lignes)

## extraction des demandes
*  distinguer le cas à une demande des cas à plusieurs dans les expérimentations
*  discuter l'impact du nombre de documents annotés pour chaque modèle
*  justifier la non utilisabilité directe des méthodes existantes
*  évaluer l'annotation des sommes d'argent par regex (vs. par CRF)
*  améliorer le modèle à base de règles:
    *  dmd: verbe introductif de demande et résultats/demande antérieurs
    *  sélection des triggers: partir du meilleurs sur les N (par ex. 100) premiers et continuer jusqu'à ce qu'il n'y ait plus d'amélioration
    *  extraction des quanta en exploitant les motifs
    *  comparer les méthodes statistiques d'apprentissage des triggers (quelle métrique est la meilleure en moyenne)
    *  si possibles traiter distinctement les jugements antérieurs et les références
*  concevoir et expérimenter la chaine de classifieur:
    *  un classifieur pour les quanta dmd, un autre pour les quanta rst, et un dernier pour les paires de quanta dmd et rst
*  concevoir et expérimenter un modèle probabiliste joint quanta-demande:
    *  modélisation log-linéaires faiblement supervisée: erreurs dans les données d'entrainement (faut positif, redondance)
    *  exploitation des triggers appris statistiquement
*  Intégrer la classification pour le sens du résultat dans les xp pour docs à 1 seule dmd


##  extraction des circonstances factuelles
*  comparaisons grossières des mth classiques et des modèles vectorielles
*  modéliser les 2 méthodes proposer de telles sortes qu'elles améliorent les résultats et qu'elles soit plus stables
*  proposer un comparatif qualitatif des techniques de labélisation de cluster
*  S'il y a du temps, proposer une approche de topic modeling en trouvant un moyen d'éliminer les termes dont on est sur qu'ils n'ont rien à voir avec les circonstances factuelles (par exemple les termes très courant - forte féquence / des void words, les termes caractéristiques des catégories de demandes)


## Démonstrateur
*  améliorer/alléger le code de l'application web
*  rajouter des fonctionnalités d'analyses descriptives (comparaisons dans le temps et l'espace)
*  Schématiser l'architecture de l'appli à l'aide de diagrammes UML (modèle des données, interaction entre composants, modèles de séquences)
*  Evaluation intrinsèque de la fiabilité du système à l'échelle des données intégrées dans la BD
    *  scénario de requête
*  Publier l'appli sur internet taj.mines-ales.fr


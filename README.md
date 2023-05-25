# ESI4-ElasticSearch

## Lundi 22/05/2023 
### TP1 :

Installation d'Elasticsearch, Kibana, Logstash à l'aide de la doc suivante (en local) : https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-windows.html 

A la suite du téléchargement du zip et de l'installation d'Elasticsearch, le "current health status" était "red" et bloquait donc le lancement d'ElasticSearch.
Le password et le token n'étaient pas générés.
A la suite de recherche, il s'avère qu'un espace insuffisant sur C:\ causait le problème. L'installation bloquait et tournait dans le vide sur le disque D:\.
Après avoir fait de l'espace sur C:\ et suite à deux lancements, ElasticSearch a finalement généré password et token.
Lancement d'ElasticSearch depuis le cmd :
- cd C:\elasticsearch-8.7.1
- .\bin\elasticsearch.bat

Installation de Kibana en deux fois (car lors de la première installation, lors du "setup", le chargement était infini). Doc : https://www.elastic.co/fr/downloads/kibana
Lancement de Kibana depuis un autre cmd :
- cd C:\kibana-8.7.1
- bin\kibana.bat

Récupération de l'URL donné par Kibana pour se connecter à l'interface web :
http://localhost:5601/app/integrations/browse

## Mardi 23/05/2023

Installation Logstash à l'aide de la doc suivante : https://www.elastic.co/fr/downloads/logstash

Téléchargement du zip
Création du fichier .conf
Lancement de logstash :
- cd C:\logstash 8.7.1
- bin/logstash -f logstash-simple.conf

Création d'un index : https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html
Dans Management -> Dev Tools

PUT /my-index-000001

Mapping : https://www.elastic.co/guide/en/elasticsearch/reference/current/explicit-mapping.html

 PUT /my-index-000001/_mapping
 {
     "properties": {
       "age": {"type":"integer"},
       "email": {"type": "binary"},
       "name": {"type": "text"}
     }
 }
 
 ### Exercices sur le mapping
 
 #### Comment Elasticsearch procède-t-il au mapping ?
 Si aucune info n'est renseignée, Elasticsearch procède au mapping de manière dynamique (type déduit automatiquement selon ce qui semble le + logique).

#### Peut-on modifier le mapping sans recréer l’index ?  
Il est possible de modifier le mapping sans recréer l'index en utilisant /_mapping :

PUT /index/_mapping
 {
   "properties" : {
   
   }
 }
### Exercices sur l’analyseur :  

#### Tentez de définir : Tokenisation et Normalisation 

Tokenisation : La tokenisation dans ElasticSearch est un processus qui divise du texte en jeton (token). Chaque jeton correspond à un mot ou partie d'un mot et cela constitue l'unité de base utilisée pour l'indexation et la recherche de texte.
Il y a plusieurs étapes à la tokenisation :
- L'analyseur : Décompose le texte en jeton (il enlève également les caractères spéciaux et espaces, et convertir les mots en minuscules)
- Suppression des mots courants qui n'apportent pas de valeur sémantique (le, la, et, de...)
- Simplification des mots pour les regrouper (selon leur racine ou autres critères)

Normalisation : La normalisation ressemble à l'analyseur présent dans la tokenisation. Cependant, celui-ci ne peut créer qu'un seul jeton et ne possède donc pas de tokeniseur. Les filtres acceptés sont donc limités.

### Les APIs :

#### Lors de la démonstration nous avons évoqué la notion d’API, desquelles avons-nous parlé ?

Nous avons parlé des API REST principalement.

### TP2 :

Suivi d'un tuto ElasticSearch sur Python pour se lancer.

## Mercredi 24/05/2023 ; Jeudi 25/05/2023

#### Tenter d’expliquer comment les données indexées sont analysées : 

Les données indexées sont analysées de la manière suivante : 
Les données sont stockées dans des index. Lorsque les données sont indexées, Elasticsearch effectue un processus d'analyse sur le texte afin de le préparer pour la recherche. L'analyse comprend plusieurs étapes : la tokenisation, la norrmalisation et le filtrage des mots.
La tokenisation transforme les mots en token, la normalisation convertit les mots pour les simplifier (mise en minuscule par exemple), le filtrage des mots supprime les mots courants jugés inutiles (le,la,de...)
Pour chaque analyse, ElasticSearch créé un index inversé pour retrouver la liste des documents qui correspondent à la requête.

### TP3 :

A – élaborez un schéma global qui permet de comprendre au mieux les concepts suivants avec Elasticsearch :  
Sharding (Diapo slide 115) 
Réplica 
Index 
Alias d’index 
Document 
Nœud (Diapo 122) 
Cluster (Diapo 121 et +) 

(Voir doc "Schéma global 3.0")

### Expliquez alors comment Elasticsearch stocke ses données et comment certaines de ces notions permettent de gagner en robustesse (en termes de sauvegarde et d’intégrité des données). 
### Terminez en résumant les fonctionnalités de mise à l’échelle… (diapo 148 et +) 

#### Comment Elasticsearch stocke ses données :
Elasticsearch stocke ses données à l'aide d'une structure appelée "index inversé". 
Lorsque des données sont indexées (=documents mis dans un index), elles sont divisées en "shards" (fragment d'index) et chaque shard est une unité de stockage indépendante. Les shards sont ensuite envoyées à travers un cluster dans le but de simplifier les opérations de recherche ou d'écriture. Le but final de cette fragmentation est d'améliorer les performances et la disponibilité

Les documents sont stockés dans un format JSON. Chaque document est composé de champs et de valeurs. Les champs peuvent contenir différents types de données tels que des chaînes de caractères, des nombres, des dates, etc.
Les données sont répliquées à travers plusieurs nœuds du cluster pour assurer la redondance et la disponibilité des données en cas de défaillance d'un nœud.

#### Comment certaines notions permettent de gagner en robustesse :
Les notions de cluster / noeuds / shards permettent de gagner en robustesse car le fait de "diviser" les données permet de ne pas avoir à faire des recherches sur l'entiereté des données lorsque l'on sait précisément où celles-ci se trouvent.
Egalement, la notion de "replica" (copie des noeuds / index / shards) permet d'augmenter la robustesse d'eslastisearch. Une redondance des données est ainsi proposée, en cas de défaillance d'un noeud, les données peuvent être récupérées à partir des copies présentes sur d'autres noeuds.
L'API Snapshot and Restore propose des fonctionnalités de sauvegarde et création de snapshots. Ces dernières permettent la création de copie sauvegardée dans un référentiel externe. Ces snapshots peuvent être utilisées pour restaurer des données.

#### Résumé des fonctionnalités de mise à l'échelle :
Les fonctionnalités de mise à l'échelle (=scaling) permettent une adaptabilité aux changements de dimensions.
Elasticsearch répartit automatiquement les shards existantes entre les noeuds (nbre de shards > nbre de noeuds).
Pour effectuer du scaling, l'unité de base est le shard. Pour optimiser les ressources, chaque shard devrait contenir entre 10 et 50 Go.
Le scaling peut également être géré via les replicas et les index.
Via Replicas : Ajout de noeuds pour créer des replicas
Via Index : Multiplier les index permettra d'augmenter la capacité de données.

### B – Testez la Scroll API :
### D’après vos recherches pourquoi l’utiliser ? Est-ce le bon paramètre de recherche pour effectuer de la recherche paginée ?  

L'utilisation de l'API Scroll permet de récupérer un grand nombre de résultats de recherche de manière efficace. Elle permet de paginer les résultats et de traiter des volumes de données plus importants.
Lorsque l'on effectue une recherche classique retournant beaucoup de résultats, ElasticSearch limite la taille des résultats renvoyés. L'API Scroll intervient à ce moment là ; pour récupérer le reste des résultats.

Il s'agit bien du bon paramètre de recherche pour effectuer de la recherche paginée. En effet, l'avantage de l'APi Scroll est qu'elle permet de ne pas avoir à paginer manuellement les résultats avec des requêtes supplémentaires (qui auraient dû etre réalisées sans l'utilisation de l'API).

### C- Kibana : 
### Quel est l’usage principal de Kibana ?  

Kibana est un outil de visualisation de données pour ElasticSearch. Il permet d'effectuer des recherches, de visualiser des documents individuels et d'agréger des résultats dans des graphes et des diagrammes.
Kibana est utilisé principalement pour explorer, analyser et visualiser des données stockées dans ElasticSearch. L'outil facilite donc le traitement des données, la création de visuels et tableaux de bords personnalisés ainsi que l'analyse des journaux.
Pour une entreprise, Kibana permettra une aide à la compréhension de données et donc une aide à la prise de décisions basées sur des informations précises et visuellement intelligibles.


### Qu’est-ce qu’un Dashboard ?  

Un Dashboard (=Tableau de bord) a pour fonction de permettre la visualisation, le suivi et l'exploitation facile de données pertinentes sous formes de chiffres, ratios et/ou graphiques. Ces indicateurs sont liés à des objectifs dans le but de prendre des décisions.
Un Dashboard sur Kibana permet de juxtaposer les vues qui ont été créées afin de visualiser en un coup d'oeil les données les plus importantes.
Kibana rend possible la création de graphiques, de cartes et de filtres pour visualiser les données d'ElasticSearch.








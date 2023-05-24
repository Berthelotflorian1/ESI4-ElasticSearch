# ESI4-ElasticSearch

## Lundi 22/05/2023 
### TP1 :

Installation d'Elasticsearch, Kibana, Logstash à l'aide de la doc suivante (en local) : https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-windows.html 

A la suite de téléchargement du zip et de l'installation d'Elasticsearch, le "current health status" était "red" et bloquait donc le lancement d'ElasticSearch.
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

## Mercredi 24/05/2023

#### Tenter d’expliquer comment les données indexées sont analysées : 

Les données indexées sont analysées de la manière suivante : 
Les données sont stockées dans des index. Lorsque les données sont indexées, Elasticsearch effectue un processus d'analyse sur le texte afin de le préparer pour la recherche. L'analyse comprend plusieurs étapes : la tokenisation, la norrmalisation et le filtrage des mots.
La tokenisation va transformer les mots en token, la normalisation va convertir les mots pour les simplifier (mise en minuscule par exemple), le filtrage des mots va supprimer les mots courants jugés inutiles (le,la,de...)
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

(Voir doc Schéma global)

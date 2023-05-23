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

Installation de Kibana en deux fois (car lors de la première installation, lors du "setup", le chargement était infini).
Lancement de Kibana depuis un autre cmd :
- cd C:\kibana-8.7.1
- bin\kibana.bat

Récupération de l'URL donné par Kibana pour se connecter à l'interface web :
http://localhost:5601/app/integrations/browse

## Mardi 23/05/2023



<p align="center">
  <a href="https://www.datatourisme.fr" target="_blank">
    <img alt="DATAtourisme" src="https://gitlab.adullact.net/adntourisme/datatourisme/api/raw/master/docs/_media/logo.png">
  </a>
</p>

<p align="center">
  La plateforme <strong>OPEN DATA</strong> de l'information touristique
</p>

<p align="center"><a href="https://www.datatourisme.fr/">https://www.datatourisme.fr</a></p>


# Environnement full-stack Docker

Ce projet contient un environnement [docker-compose](https://docs.docker.com/compose/) contenant les
services suivants :

* La base de données [Blazegraph](http://www.blazegraph.com)
* L'[API DATAtourisme](https://gitlab.adullact.net/adntourisme/datatourisme/api) associée à :
    * Un point d'accès HTTP GraphQL
    * [GraphiQL](https://github.com/graphql/graphiql), une interface visuelle de conception de requête
    * [Voyager](https://github.com/APIs-guru/graphql-voyager), un visualisateur de schéma sous forme de graphe.

Ce stack vous permet de mettre rapidement en place un environnement de restitution des données touristiques
téléchargées à partir de la [plateforme diffuseur](https://diffuseur.datatourisme.fr).

## Utilisation

### Pré-requis

Pour utiliser l'environnement, doivent être installés et configurés :

* [Docker-ce](https://docs.docker.com/engine/installation/)
* [docker-compose](https://docs.docker.com/compose/install/)

Par défaut, vous devrez rendre disponible les ports 8080 et 9999.

### Récupération d'un fichier de données

Pour vous servir de l'environnement, vous devez d'abord récupérer des données touristiques :

1. Connectez-vous à la [plateforme diffuseur](https://diffuseur.datatourisme.fr). Il sera nécessaire de
créer un compte s'il s'agit de votre première connexion.
2. Créez et configurez un flux de données à l'aide de l'éditeur visuel de requête.
3. Configurez le flux pour utiliser un format **compatible avec l'API** :
    * RDF-XML
    * Turtle
    * NT
4. Téléchargez le fichier du flux une fois celui-ci disponible.

Le fichier ainsi obtenu doit être copié dans le répertoire `dataset/kb/data` pour être chargé dans la base de
données lors de la création du conteneur Docker.

À titre d'information, voici comment se décompose le dossier `dataset` :

| Répertoire                       | description                                     |
| -------------------------------- |-------------------------------------------------|
| dataset/                         | Répertoire décrivant la configuration et les données à charger dans le namespace |
| &nbsp;&nbsp;&nbsp;/kb/           | Namespace pour les données (kb étant le namespace par défaut) |
| &nbsp;&nbsp;&nbsp;/data/         | Répertoire où copier les données sémantiques (ttl, nt, n3, xml, gz...) |
| &nbsp;&nbsp;&nbsp;/RWSTore.properties    | Configuration de la base de données pour ce namespace namespace |

> **Si vous choisissez de charger la base complète il faut augmenter la mémoire Java dédiée à la base de données. Pour ce faire vous pouvez éditer le fichier `docker-compose.yml` et changer la valeur de `JAVA_OPTS` en `-Xms2g -Xmx3g`**

### Lancement de l'environnement

Vous pouvez maintenant lancer l'environnement :

```
$ docker-compose up
```

Ces commandes lanceront deux serveurs :
 - un serveur [Blazegraph](https://www.blazegraph.com/) sur le port 9999 chargé de vos données.
 - un serveur incluant l'[API DATAtourisme](https://gitlab.adullact.net/adntourisme/datatourisme/api) sur le port 8080.

Vous pouvez accèder aux interfaces suivantes :

* Interface d'administration Blazegraph : http://localhost:9999
* Point d'accès HTTP GraphQL : http://localhost:8080
* GraphiQL : http://localhost:8080/graphiql
* Voyager : http://localhost:8080/voyager

### Utilisation de l'API

Pour utiliser l'API, reportez vous à la [documentation officielle](https://datatourisme.frama.io/api/#/).

### Mise à jour des données

Lorsque vous souhaitez mettre à jour les données de votre environnement, vous devez procéder ainsi :

1. Stoppez l'environnement s'il est actif : `docker-compose stop`
1. Téléchargez le nouveau fichier de donnée et remplacez le dans **dataset/kb/data**
2. Supprimez le conteneur Blazegraph : `docker-compose rm blazegraph`
3. Relancez l'environnement : `docker-compose up`

## Liens

* [API DATAtourisme](https://gitlab.adullact.net/adntourisme/datatourisme/api)
* [Documentation de l'API](https://datatourisme.frama.io/api)
* [Plateforme diffuseur](https://diffuseur.datatourisme.fr)
* Centre de support

## Licence

MIT License
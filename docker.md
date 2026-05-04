# Docker

## Préparatifs

Utilisation de l'extension `ms-azuretools.vscode-containers` dans Codium.

## Exercice 1 - Conteneur postgres

### Enoncé

Créer un conteneur basé sur l'image Postgres.

Faire en sorte que la connexion avec la base `postgres` soit possible depuis un autre conteneur en utilisant le client `psql` de la même image Postgres.

Le second conteneur doit être temporaire/éphémère. A la fin de l'exécution du processus psql, le conteneur doit être détruit.

L'utilisation de la commande `docker exec` est interdit.

La connexion depuis le second conteneur vers le premier conteneur doit se faire via un alias DNS.

![](./images/exercice1.png)

### Solution

Aide : utiliser la commande `docker run` et consulter la [documentation officielle](https://hub.docker.com/_/postgres) de l'image Docker postgres

Pour rappel, la commande `docker run` effectue l'équivalent d'un appel séquentiel à ces 3 commandes

```bash
1. docker pull
2. docker create
3. docker start
```

#### Conteneur 1 - Base de données

Créer le conteneur hébergeant le processus d'instance de postgres

```bash
docker run --name exercice1 -e POSTGRES_PASSWORD=password -d postgres:latest
```

Lister tous les conteneurs actifs

```bash
docker ps
```

Lister tous les conteneurs actifs/inactifs

```bash
docker ps -a
```

Récupérer au format json toutes les infos d'un conteneur spécifique

```bash
docker inspect exercice1
```

Démarrer un second processus dans le conteneur de l'instance pour vérifier que la connexion à l'instance fonctionne localement

```bash
docker exec exercice1 psql -h localhost -U postgres -c "SELECT NOW()"
```

Démarrer également un processus interactif dans le conteneur de l'instance active 

```bash
docker exec -it exercice1 psql -h localhost -U postgres
```

#### Conteneur 2 - Client base de données


Récupérer l'adresse IP de notre base de données démarrée dans le conteneur `exercice1`.

```bash
docker inspect exercice1 --format='{{.NetworkSettings.Networks.bridge.IPAddress}}'
```

```bash
docker run -it --rm postgres psql -h 172.17.0.2 -U postgres
```

Connaître les réseaux auxquels un conteneur est rattaché en passant par la commande inspect

```bash
docker inspect exercice1 --format='{{.NetworkSettings.Networks}}'
```

Créer un réseau personnalisé

```bash
docker network create exercice1_network
```

Associer le conteneur actif à ce réseau

```bash
docker network connect exercice1_network exercice1 --alias db
```

Relancer la commande psql dans un nouveau conteneur cette fois associé à notre nouveau réseau

```bash
docker run -it --rm --network exercice1_network postgres psql -h db -U postgres -c "SELECT NOW()"
```
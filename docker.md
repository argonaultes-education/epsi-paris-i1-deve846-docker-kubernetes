# Docker

## Exercice 1 - Conteneur postgres

### Enoncé

Créer un conteneur basé sur l'image Postgres.

Faire en sorte que la connexion avec la base postgres soit possible depuis un autre conteneur en utilisant le client `psql` de la même image Postgres.

Le second conteneur doit être temporaire/éphémère. A la fin de l'exécution du processus psql, le conteneur doit être détruit.

L'utilisation de la commande `docker exec` est interdit.

La connexion depuis le second conteneur vers le premier conteneur doit se faire via un alias DNS.

### Solution


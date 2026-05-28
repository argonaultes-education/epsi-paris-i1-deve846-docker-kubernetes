# Kubernetes

Les 3 fonctionnalités essentielles attendues rendues par un cluster Kubernetes :

* self healing
* automated rollback
* automated scaling

## Exercice 1 - Installation d'un cluster en local


Installer [minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download)

```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

Tester que minikube est correctement installé

```bash
minikube version
```

Créer et démarrer un cluster kubernetes

```bash
minikube start --cpus='2' --memory='4g' --driver='docker'
```

Vérifier l'existence du nouveau conteneur et sa consommation

```bash
docker stats
```

Pour interagir avec le cluster : kubectl. Pour l'installer, suivre la [documentation](https://kubernetes.io/docs/tasks/tools/#kubectl).

Tester l'installation de kubectl

```bash
kubectl version
```

Le résultat attendu doit correspondre à l'écran ci-dessous

```
Client Version: v1.35.4
Kustomize Version: v5.7.1
Server Version: v1.35.1
```

Lister les ressources présentes dans le cluster par défaut

```bash
kubectl get pods -A
```

Vue d'ensemble logique d'un cluster kubernetes

![](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

## Exercice 2 - Créer notre premier pod

```bash
kubectl run nginx --image=nginx:latest --port=80
```

Lister les pods présents et leur état dans le cluster kubernetes

```bash
kubectl get pods
```

Lister tous les namespaces avec la commande

```bash
kubectl get namespace
```

Pour créer des ressources, utiliser l'une des méthodes suivantes :

* impérative
* déclarative

Avec minikube, afficher le tableau de bord de supervision

```bash
minikube dashboard
```

Démarrer un nouveau processus interactif de shell (Se connecter) pour tenter d'arrêter le processus nginx.

```bash
kubectl exec nginx -c nginx -it -- bash
```

Détruire le pod

```bash
kubectl delete pod nginx
kubectl delete pod/nginx
```


## Exercice 3 - Créer un pod dont le conteneur s'éteind au bout de x secondes

Choisir l'image de votre choix `debian:latest`, éventuellement créer votre propre image ou bien lancer une commande au démarrage.

```bash
kubectl run testsleep --image=debian:latest -- sleep 5
```

Résultat attendu

```bash
kubectl get pods
```

```bash
NAME        READY   STATUS      RESTARTS      AGE
testsleep   0/1     Completed   3 (55s ago)   92s
```

## Exercice 4 - Self-Healing avec les déploiements

Créer maintenant une ressource de type déploiement

```bash
kubectl create deployment nginx --image=nginx:latest
```

Faire varirer le nombre de pods associés à ce déploiement

```bash
kubectl scale deployment/nginx --replicas=2
```

## Exercice 5 - Rendre accessible le conteneur depuis l'exterieur du cluster




## Fin

Arrêter minikube et détruire les ressources associées

```bash
minikube stop
minikube delete
``` 
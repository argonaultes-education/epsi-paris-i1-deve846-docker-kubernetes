# Kubernetes

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


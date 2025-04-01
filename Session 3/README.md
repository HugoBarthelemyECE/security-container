# Sécurité des orchestrateur

## Déployer un Cluster Kubernetes avec Kind

Creating cluster with kind config file

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: worker
- role: worker
```

We use the following command

```
kind create cluster --config config.yaml
```

La commande `get nodes` permet de lister les nodes créés

```
kind-control-plane
kind-worker
kind-worker2
kind-control-plane2
```

La commande suivante permet de lister les namespaces dans kubernetes

```
kubectl get namespaces
```

```
NAME                 STATUS   AGE
default              Active   4m
kube-node-lease      Active   4m
kube-public          Active   4m
kube-system          Active   4m
local-path-storage   Active   3m47s
```

La version de kubenernetes utilisée est la 1.32.2

## Expérimentation des RBAC

On peut créer un namespace kubernetes avec la commande suivante.

```
kubectl create ns test-rbac
```

On déploie ensuite un pod dans le namespace nouvellement créé. Dans ce pod on retrouve un container avce une image du serveur web Nginx

```
kubectl apply -f mon-pod.yaml
```

On peut afficher les logs du pod avec la commande suivante en précisant le namespace associé

```
kubectl logs nginx -n test-rbac
```

Pour créer un rôle permettant de lire les pods dans le namespace test-rbac on associe les verbs `get` et `list` à ce rôle.

On peut afficher ce rôle pour en voir les caractéristiques

```
kubectl describe role pod-reader -n test-rbac
```

```
Name:         pod-reader
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  pods       []                 []              [get list]
```

Il faut ensuite créér un role binding et l'appliquer

Pour créér un nouvel utilisateur on va d'abord générer une clé RSA afin de générer un certificat que l'on va signer avec les identifiants de kubernetes

Il faut alors ajouter l'utilisateur dans le namespace `test-rbac`

```
kubectl config set-credentials titi --client-certificate=titi.crt --client-key=titi.key
```

Créér un contexte et basculer le contexte

```
kubectl config set-context titi-context --cluster=kind-kind --namespace=test-rbac --user=titi 
```

```
kubectl config use-context titi-context
```

On peut à présent toujours lister les pods. Cependant lorsque l'on essaie de créér un pod, l'API nous retourne une erreur car on n'a pas les droits.

```
error when patching "mon-pod.yaml": pods "nginx" is forbidden: User "titi" cannot patch resource "pods" in API group "" in the namespace "test-rbac"
```

## Scanner un Cluster Kubernetes avec Kube-Bench

```
kubectl logs kube-bench-ckcrq
```

## Détection et alerte d'intrusions dans kubernetes avec l'outil Falco

Création d'un namespace Falco dans kubernetes

```
kubectl create ns falco
```

Les services sont un peu long à démarrer on peut suivre le statut avec

```
kubectl get pods -n falco
```




# Session 2 : Bonnes pratiques pour sécuriser les containers


### Éviter l’Exposition Involontaire de Ports

Lancement d'un container avec restriction de ports : 

`docker run -d -p 8080:80 nginx`

Cette commande lance un conteneur Nginx en arrière-plan et mappe le port 8080 de l'hôte au port 80 du conteneur.

Nous allons utiliser `netstat -tuln` pour vérifier si le port est exposé. Le port 8080 est bien exposé sur la machine hôte c'est d'ici qu'on peut accèder au serveur.

```
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN
```

### Restreindre les permissions d’accès aux fichiers sensibles

On peut monter le fichier des mots de passe du système de l'hôte dans le volume du container comme ceci. `ro` signifie read only. On devrait avoir uniquement les accès en lecture.

`docker run -it --rm -v /etc/shadow:/mnt/shadow:ro alpine sh`

Comme on peut le voir, il nous est impossible d'écrire dans le fichier. On peut juste l'afficher.

```
/ # echo test > /mnt/passwd
sh: can't create /mnt/passwd: Read-only file system
```

### Audit de container

Le score pour l'audit du host et de l'image vulnérables sont -2.

### Stockage des secret

Vault nous permet de stocker des secrets. Une interface graphique nous permet d'intéragir facilement avec l'application.

On peut créer un ACL avec le contenu suivant pour créer un chemin d'accès.

```
path "containers/mon-secret" {
  capabilities = ["read", "list"]
}
```

La connexion à vault se fait avec la commande curl

`curl --request POST --data '{"password": "zklhjrid"}' http://172.17.0.1:8200/v1/auth/userpass/login/aurel`

On obtient un jeton d'accès que l'on utilise pour extraire le secret

`curl --header "X-Vault-Token: <jeton>" http://172.17.0.1:8200/v1/containers/mon-secret`

### Trouver la clé

La commande `docker history` permet d'afficher l'historique des commandes exécutées dans l'image.

`docker history --no-trunc ety92/demo:v1`

On trouve dans l'historique une requête web vers google avec la clé API cherchée.

```
curl -H "API-Key: U-never-will-saw-that" -L google.com
```

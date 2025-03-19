# Session 2 : Bonnes pratiques pour sécuriser les containers


## Éviter l’Exposition Involontaire de Ports

Lancement d'un container avec restriction de ports : 

`docker run -d -p 8080:80 nginx`

Cette commande lance un conteneur Nginx en arrière-plan et mappe le port 8080 de l'hôte au port 80 du conteneur.

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
6e909acdb790: Pull complete
5eaa34f5b9c2: Pull complete
417c4bccf534: Pull complete
e7e0ca015e55: Pull complete
373fe654e984: Pull complete
97f5c0f51d43: Pull complete
c22eb46e871a: Pull complete
Digest: sha256:124b44bfc9ccd1f3cedf4b592d4d1e8bddb78b51ec2ed5056c52d3692baebc
Analyser une image avec Grype
Status: Downloaded newer image for nginx:latest
0e0878a438eac84b185575684b7498c591b3f9a9c69a4d9e1efa38d70edae8c3
```

Nous allons utiliser `netstat -tuln` pour vérifier si le port est exposé : 

```
mettre résultat
```

## Restreindre les permissions d’accès aux fichiers sensibles



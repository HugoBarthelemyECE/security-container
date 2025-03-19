# security-container

## Container simple

La commande suivante permet de lancer un container de test.

`docker run --rm hello-world`

Le container affiche un simple message dans le terminal pour nous informer que tout fonctionne bien. L'option `--rm` permet de supprimer le container après l'execution.

`hello-world` est une image docker qui est récupérée depuis Docker Hub

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Container interactif

La commande `docker run -it --rm alpine sh` lance un container interacrif avec les otions `-it` ce qui nous permet d'exécuter des commandes dans un shell. Ici, l'image est Alpine qui est une distribution Linux légère. En quelque sorte on rentre dans le container.

Il est par exemple possible de lister le contenu du dossier racine.

```
latest: Pulling from library/alpine
f18232174bc9: Pull complete 
Digest: sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c
Status: Downloaded newer image for alpine:latest
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # 
```

## Analyse des ressources

La commande suivante permet de lancer un container en arrière plan. Nginx est un serveur web.

`docker run -d --name test-container nginx`

Il est ensuite possible d'afficher les informations sur les ressources du système qui sont les suivantes. Ici, l'image utilise 13MiB de mémoire.

```
CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O    PIDS
e63ac78585e3   test-container   0.00%     12.87MiB / 14.92GiB   0.08%     17.6kB / 0B   0B / 831kB   17
```

## Capacités d'un container


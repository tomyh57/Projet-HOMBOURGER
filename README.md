# HOMBOURGER_Tomy_Projet-E3-3MSCOR
Voici le repository de HOMBOURGER Tomy pour le Projet E3 3MSCOR. Je suis en groupe avec KADI Soulaymane, FIGAROLI Romain, AHMADI Rateb et BERNARD Jonathan

## Structure du projet

```python
< projet_final >
   |
   | -- django-argon-dashboard_jb
   | -- django-coreui_jb
   | -- django-datta-able_jb
   | -- django-material-dashboard_jb
   | -- django-pixel_jb
   | -- django-soft-ui-design_jb
   | -- nginx/
   |    |-- html
   |    |-- appseed-app.conf
   |    |-- Dockerfile
   |-- docker-compose.yml
```
## Schéma du projet

![image]


## Outils utilisés

![Capture d'écran 2024-12-11 095925](
## Téléchargement des applications



## Téléchargement du site HTML


## Paramétrage des applications

J’ai supprimé le fichier docker-compose et le dossier nginx de chaque application pour n’en garder qu’un seul de chaque. Le docker-compose permettra de déployer toutes les applications en une commande et le nginx permettra de réunir toutes les applications derrière un 
seul reverse proxy.


Le docker compose :

```yml
version: '3.8'
services:
  django-argon-dashboard_jb:
    container_name: django-argon-dashboard_jb
    restart: always
    ports :
      - "1001:1001"
    build: "./django-argon-dashboard_jb"
    networks:
      - db_network
      - web_network
  django-coreui_jb:
    container_name: django-coreui_jb
    restart: always
    build: "./django-coreui_jb"
    networks:
      - db_network
      - web_network
  django-datta-able_jb:
    container_name: django-datta-able_jb
    restart: always
    build: "./django-datta-able_jb"
    networks:
      - db_network
      - web_network
  django-material-dashboard_jb:
    container_name: django-material-dashboard_jb
    restart: always
    build: "./django-material-dashboard_jb"
    networks:
      - db_network
      - web_network
  django-pixel_jb:
    container_name: django-pixel_jb
    restart: always
    build: "./django-pixel_jb"
    networks:
      - db_network
      - web_network
  django-soft-ui-design_jb:
    container_name: django-soft-ui-design_jb
    restart: always
    build: "./django-soft-ui-design_jb"
    networks:
      - db_network
      - web_network
  nginx_jb:
    container_name: nginx_jb
    restart: always
    image: "nginx:stable-alpine-slim"
    ports:
      - "80:80"
      - "8082:8082"
      - "8083:8083"
      - "8084:8084"
      - "8085:8085"
      - "8086:8086"
      - "8087:8087"
    volumes:
      - ./nginx/html:/usr/share/nginx/html
      - ./nginx:/etc/nginx/conf.d
    networks:
      - web_network
    depends_on: 
      - django-coreui_jb
      - django-datta-able_jb
      - django-material-dashboard_jb
      - django-pixel_jb
      - django-soft-ui-design_jb
networks:
  db_network:
    driver: bridge
  web_network:
    driver: bridge
 ```

Lignes du appseed-app.conf de NGINX :

```python
upstream django-coreui_jb {
    server django-coreui_jb:1002;
}

server {
    listen 8082;
    server_name localhost;

    location / {
        proxy_pass http://django-coreui_jb;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
upstream django-datta-able_jb {
    server django-datta-able_jb:1003;
}
server {
    listen 8083;
    server_name localhost;

    location / {
        proxy_pass http://django-datta-able_jb;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
upstream django-material-dashboard_jb {
    server django-material-dashboard_jb:1004;
}

server {
    listen 8084;
    server_name localhost;

    location / {
        proxy_pass http://django-material-dashboard_jb;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

upstream django-pixel_jb {
    server django-pixel_jb:1005;
}

server {
    listen 8085;
    server_name localhost;

    location / {
        proxy_pass http://django-pixel_jb;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

upstream django-soft-ui-design_jb {
    server django-soft-ui-design_jb:1006;
}

server {
    listen 8086;
    server_name localhost;

    location / {
        proxy_pass http://django-soft-ui-design_jb;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

server {
    listen 8087;
    server_name localhost;

    root /usr/share/nginx/html; # Chemin vers vos fichiers HTML
    index index.html; # Page d'accueil de votre application

    location / {
        try_files $uri /index.html; # Pour assurer le support des routes SPA (Single Page Applications)
    }

    error_page 404 /404.html;
}
```
Modification des fichiers gunicorn.py pour chaque application sauf HTML car elle ne possède pas ce fichier :

```
App 1 : bind = '0.0.0.0:1001'
App 2 : bind = '0.0.0.0:1002'
App 3 : bind = '0.0.0.0:1003'
App 4 : bind = '0.0.0.0:1004'
App 5 : bind = '0.0.0.0:1005'
App 6 : bind = '0.0.0.0:1006'
```

## Optimisation des Dockerfile en prenant des images plus légères et en réunissant des commandes :

```Dockerfile
FROM python:3.10-alpine

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

COPY requirements.txt .
# install python dependencies
RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt

COPY . .

# running migrations
RUN python manage.py migrate

# gunicorn
CMD ["gunicorn", "--config", "gunicorn-cfg.py", "core.wsgi"]

```

Dockerfile de l'application HTML :

```
FROM nginx:stable-alpine-slim

# Copie des fichiers HTML dans le dossier par défaut de Nginx
COPY ../html /usr/share/nginx/html

# Copie de la configuration personnalisée
COPY ./appseed-app.conf /etc/nginx/conf.d/default.conf
```

Modification du dossier de l’application HTML en conséquence, je mets aussi le dossier HTML dans le dossier nginx pour faciliter son déploiement et son accès. Pour mon application HTML il suffit juste de prendre une image NGINX optimisé, copier les fichiers HTML dans le dossier NGINX du container pour que l'application fonctionne.



## Build des applications 
```python
cd dossier/contenant/le docker-compose
docker-compose up
```
Preuve de lancement des containers :



Preuves du fonctionnement des appplications passant par le reverse proxy :



Preuve du fonctionnement de l'appplication avec accès interne :


## Push des applications vers Dockerhub
J'ai d'abord créé mon repository Docker sur le site :



Puis j'ai répété ces commandes pour chaque image :
```python
docker tag <id image> <username>/<nom du repository docker>:<nom du tag>
docker push <username>/<nom du repository>:<nom du tag>

Par exemple : docker tag c71b491bf6fc jonathan57/projet_bernard:projet_final-django-datta-able_jb
              docker push jonathan57/projet_bernard:projet_final-django-datta-able_jb
```
Preuves de fonctionnement du push vers Dockerhub :


## Questions DevOPS

Qu'est-ce que le DevOps ?
Le DevOps, c’est un ensemble de pratiques qui combinent le développement logiciel. Le but principal est d’améliorer la collaboration entre les équipes pour livrer des projets plus rapidement, tout en garantissant la qualité.

En quoi le devops vous a aidé dans cette mission ?
Le DevOps nous a permis de travailler de façon plus organisée et de gagner du temps.

Votre chef de projet vous demande de comparer l'utilisation de Docker avec des machines virtuelles. Que répondrez-vous ?
Docker est plus léger que les machines virtuelles. Une VM embarque tout un système d’exploitation, ce qui consomme beaucoup de ressources.

À l'aide de vos connaissances personnelles, citer un ou plusieurs autres outils similaires à docker qui pourrait aider votre équipe à livrer et maintenir ce projet client.
Il existe Kubernetes et PodMan comme outils similaires à Docker.







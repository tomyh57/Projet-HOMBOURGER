# BERNARD_Jonathan_Projet-E3-3MSCOR
Voici le repository de BERNARD Jonathan pour le Projet E3 3MSCOR. Je suis en groupe avec KADI Soulaymane, FIGAROLI Romain, AHMADI Rateb et HOMBOURGER Tomy

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

![image](https://github.com/user-attachments/assets/0d0c4cca-8eec-4e1d-b37a-06bfb06ec9fe)


## Outils utilisés

![Capture d'écran 2024-12-11 095925](https://github.com/user-attachments/assets/4bde3793-7173-4829-ab28-c7ba7281210e)

## Téléchargement des applications

![Capture d'écran 2024-12-11 100000](https://github.com/user-attachments/assets/d55d561d-788e-492d-ab01-31064d5b90dd)
![Capture d'écran 2024-12-11 100011](https://github.com/user-attachments/assets/4645eb78-cc07-459a-a0ee-acbd15e3f292)
![Capture d'écran 2024-12-11 100027](https://github.com/user-attachments/assets/c309497a-441f-457e-a684-cba52e465caa)
![Capture d'écran 2024-12-11 100043](https://github.com/user-attachments/assets/b5fd7718-6793-4056-9a4a-b4862977f5a7)
![Capture d'écran 2024-12-11 100103](https://github.com/user-attachments/assets/89476c5d-aa28-426e-b09a-631d3dcf1dc2)
![Capture d'écran 2024-12-11 100142](https://github.com/user-attachments/assets/beaf1672-98cc-4546-99fd-75d3b94feb12)

## Téléchargement du site HTML

![Capture d'écran 2024-12-11 095801](https://github.com/user-attachments/assets/7f76ab05-01d3-43fb-a3b0-003391fbaa01)

## Paramétrage des applications

J’ai supprimé le fichier docker-compose et le dossier nginx de chaque application pour n’en garder qu’un seul de chaque. Le docker-compose permettra de déployer toutes les applications en une commande et le nginx permettra de réunir toutes les applications derrière un 
seul reverse proxy.

![image](https://github.com/user-attachments/assets/be45a7e9-f0d5-4df4-afa1-f407908f44c3)

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

![image](https://github.com/user-attachments/assets/1d776a03-8a03-48e2-ae7e-10ff521c0b0f)

## Build des applications 
```python
cd dossier/contenant/le docker-compose
docker-compose up
```
Preuve de lancement des containers :

![image](https://github.com/user-attachments/assets/ff36692d-f018-42e8-bb99-35860dda18d1)

Preuves du fonctionnement des appplications passant par le reverse proxy :

![image](https://github.com/user-attachments/assets/076e6969-b2bb-4a8d-a37f-348249f0cb70)
![image](https://github.com/user-attachments/assets/928035a9-6585-4b01-88ea-4584b094aad5)
![image](https://github.com/user-attachments/assets/50e3d771-9edc-4b81-ad52-731f8ca21e1c)
![image](https://github.com/user-attachments/assets/14994b7e-c9df-4eef-915a-746531cf34db)
![image](https://github.com/user-attachments/assets/24336fab-6a21-406c-b2a3-161cb78aab0c)
![image](https://github.com/user-attachments/assets/96c7dd1b-a1c0-4b2b-9c82-e48fd74a0107)

Preuve du fonctionnement de l'appplication avec accès interne :

![image](https://github.com/user-attachments/assets/0be2296e-da4e-4f6a-af9e-b4b75aca84e4)

## Push des applications vers Dockerhub
J'ai d'abord créé mon repository Docker sur le site :

![image](https://github.com/user-attachments/assets/fc29367b-d7ab-43e7-bd25-b187bf5cee77)

Puis j'ai répété ces commandes pour chaque image :
```python
docker tag <id image> <username>/<nom du repository docker>:<nom du tag>
docker push <username>/<nom du repository>:<nom du tag>

Par exemple : docker tag c71b491bf6fc jonathan57/projet_bernard:projet_final-django-datta-able_jb
              docker push jonathan57/projet_bernard:projet_final-django-datta-able_jb
```
Preuves de fonctionnement du push vers Dockerhub :
![image](https://github.com/user-attachments/assets/12802564-4c3f-441e-911b-7b0d46875949)
![image](https://github.com/user-attachments/assets/6a0cb914-af45-4a08-9776-6e6d73838fd5)

## Questions DevOPS

Qu'est-ce que le DevOps ?
Le DevOps, c’est un ensemble de pratiques qui combinent le développement logiciel. Le but principal est d’améliorer la collaboration entre les équipes pour livrer des projets plus rapidement, tout en garantissant la qualité.

En quoi le devops vous a aidé dans cette mission ?
Le DevOps nous a permis de travailler de façon plus organisée et de gagner du temps.

Votre chef de projet vous demande de comparer l'utilisation de Docker avec des machines virtuelles. Que répondrez-vous ?
Docker est plus léger que les machines virtuelles. Une VM embarque tout un système d’exploitation, ce qui consomme beaucoup de ressources.

À l'aide de vos connaissances personnelles, citer un ou plusieurs autres outils similaires à docker qui pourrait aider votre équipe à livrer et maintenir ce projet client.
Il existe Kubernetes et PodMan comme outils similaires à Docker.







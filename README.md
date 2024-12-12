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


![schéma](https://github.com/user-attachments/assets/b6416393-7cf0-4f2b-abdc-2d5efa531353)


## Outils utilisés
![image](https://github.com/user-attachments/assets/85008dd5-1a01-44d9-bee1-5034c78122ef)

## Téléchargement des applications

![image](https://github.com/user-attachments/assets/32a6aac4-4498-4a33-be8f-142c17740de8)
![image](https://github.com/user-attachments/assets/3a7d90fd-ad4c-4f36-9f2a-b2da30a407d3)
![image](https://github.com/user-attachments/assets/deba389b-34fe-4a67-a4f9-e441e20dc1c3)
![image](https://github.com/user-attachments/assets/2265fba6-7272-4e82-9662-d166e49995c4)
![image](https://github.com/user-attachments/assets/49779e9d-26e4-4a82-ad3a-9f8f6ced66cc)



## Téléchargement du site HTML

![image](https://github.com/user-attachments/assets/0f8fe125-2600-491f-a66a-fb8c9c3f4956)

## Paramétrage des applications

J’ai supprimé le fichier docker-compose et le dossier nginx de chaque application pour n’en garder qu’un seul de chaque. Le docker-compose permettra de déployer toutes les applications en une commande et le nginx permettra de réunir toutes les applications derrière un 
seul reverse proxy.

![image](https://github.com/user-attachments/assets/fb6520e6-67da-479c-8e3c-9e099e3a8803)


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

![image](https://github.com/user-attachments/assets/7a616e31-eec4-4c6b-815a-5cfd992baf06)



## Build des applications 
```python
cd dossier/contenant/le docker-compose
docker-compose up
```
Preuve de lancement des containers :

![image](https://github.com/user-attachments/assets/525e2b76-eedd-4e79-acd4-6d93a1b9386d)



Preuves du fonctionnement des appplications passant par le reverse proxy :

![image](https://github.com/user-attachments/assets/111c427c-a79b-4d6c-990b-6cd983da693a)
![image](https://github.com/user-attachments/assets/98d66827-baa9-40f4-bd13-df57b7fa98c4)
![image](https://github.com/user-attachments/assets/461f4eac-bddd-423e-9ca2-a90a5f58a176)
![image](https://github.com/user-attachments/assets/a42ac74b-f7a9-4efd-aefc-54675ad1f14e)
![image](https://github.com/user-attachments/assets/c9edd65f-138e-4b27-b424-61d5e883e053)
![image](https://github.com/user-attachments/assets/c07f51dc-f32a-4b45-b4b6-c320bd1d822a)



Preuve du fonctionnement de l'appplication avec accès interne :

![image](https://github.com/user-attachments/assets/800d6d87-046c-41ab-b2f7-f98096aa1f01)


## Push des applications vers Dockerhub
J'ai d'abord créé mon repository Docker sur le site :

![image](https://github.com/user-attachments/assets/a20f3d5e-0e05-480c-8b0b-2f1b6b8fcb44)


Puis j'ai répété ces commandes pour chaque image :
```python
docker tag <id image> <username>/<nom du repository docker>:<nom du tag>
docker push <username>/<nom du repository>:<nom du tag>

```
Preuves de fonctionnement du push vers Dockerhub :
![image](https://github.com/user-attachments/assets/3f46a64b-fc0c-4723-9e1c-0dc6f52331f5)


## Questions DevOPS

Qu'est-ce que le DevOps ?
Le DevOps, c’est un ensemble de pratiques qui combinent le développement logiciel. Le but principal est d’améliorer la collaboration entre les équipes pour livrer des projets plus rapidement, tout en garantissant la qualité.

En quoi le devops vous a aidé dans cette mission ?
Le DevOps nous a permis de travailler de façon plus organisée et de gagner du temps.

Votre chef de projet vous demande de comparer l'utilisation de Docker avec des machines virtuelles. Que répondrez-vous ?
Docker est plus léger que les machines virtuelles. Une VM embarque tout un système d’exploitation, ce qui consomme beaucoup de ressources.

À l'aide de vos connaissances personnelles, citer un ou plusieurs autres outils similaires à docker qui pourrait aider votre équipe à livrer et maintenir ce projet client.
Il existe Kubernetes et PodMan comme outils similaires à Docker.







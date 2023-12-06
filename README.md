# TP Docker - Containerisation d'une Application React.js, Spring Boot et MySQL

## Prérequis
Assurez-vous d'avoir les outils suivants installés sur votre machine :
- Docker
- Docker Compose

## Étapes

### 1. Structure du Projet
Organisez votre projet de la manière suivante :
/your_project_root
|-- backend
|   |-- Dockerfile
|   |-- src
|   |-- target
|-- frontend
|   |-- Dockerfile
|   |-- public
|   |-- src
|   |-- package.json
|-- docker-compose.yml
|-- README.md

### 2. Dockerfile pour le Frontend (React.js)
Créez un fichier Dockerfile dans le dossier frontend :
```Dockerfile
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
### 3. Configuration de Docker Compose
Créez un fichier docker-compose.yml à la racine du projet :
```
version: "3"
services:
  springboot-app:
    image: springboot-app
    build: ./backend
    ports:
      - 8080:8080
    environment:
      MYSQL_HOST: mysqldb
      MYSQL_USER: root
      MYSQL_PASSWORD: root
      MYSQL_PORT: 3306

  mysqldb:
    image: mysql
    ports:
      - 3307:3306
    environment:
      MYSQL_DATABASE: test
      MYSQL_ROOT_PASSWORD: root

  react-frontend:
    image: frontend
    build: ./frontend
    ports:
      - 3000:80
```
### 4. Construire et Exécuter les Conteneurs
Lancez la commande suivante dans votre terminal :
```
docker-compose up
```
### 5. Accéder à l'application
Ouvrez votre navigateur et tapez l'URL suivante :
```
http://localhost:3000/
```






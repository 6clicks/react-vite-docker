# React Vite Docker Starter

## Description
Ce projet est un modèle de démarrage rapide pour une application React utilisant Vite et Docker, Sans avair besoin de Node.JS sur votre machine
permettant un développement rapide et cohérent sur différents environnements.

## Prérequis
- Docker (version 20.10 ou supérieure)
- Docker Compose (version 1.29 ou supérieure)


## Installation Rapide

`mkdir my-app`

`docker run -it --rm -v ${PWD}:/app -w /app node:18-alpine npx create-vite my-app`

`docker run -it --rm -v ${PWD}/my-app:/app -w /app node:18-alpine npm install`

dans le fichier vite.config.js ou .ts remplacer tout par:
```jsx
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
  server: {
    watch: {
      usePolling: true,
    },
    host: "0.0.0.0",
    port: 5173,
  },
  plugins: [react()],
});

```

`docker-compose up --build -d`

`http://localhost:5173/`


## Installation détailée

L’idée est de créer un Docker pour faire tourner notre app sans devoir installer nodeJS ni npm sur notre machine. 

**A. Crée sur votre machine un Dossier pour héberger votre futur app** 
(idéalement dans le `C:/project.` )


**B. Créer un dossier**
 “`my-app`” -> ( `mkdir my-app`)

**C. 1ère ligne de commande** 
On lance une commande qui va créer un container Docker temporaire avec Node 18 (peu importe la version locale de node) pour lui lancer le `npx create-vite` dans le dossier `my-app`

```docker
docker run -it --rm -v ${PWD}:/app -w /app node:18-alpine npx create-vite my-app
```

Il vous demandera peu être ceci taper→ Y 

```docker
Need to install the following packages:
  create-react-app@5.0.1
Ok to proceed? (y)
```

Faire le choix de notre techno ? react → javascript


**D. 2ᵉ ligne de commande**

On lance à nouveau une commande qui va créer un container Docker temporaire pour installer les dépendances avec `npm install` directement dans le dossier my-app. 

```Docker
docker run -it --rm -v ${PWD}/my-app:/app -w /app node:18-alpine npm install

```
**E modification des port dans la config de vite **
dans le fichier vite.congig.js ou (.ts) dans le dossier my-app ont remplace le tout par
```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
  server: {
    watch: {
      usePolling: true,
    },
    host: "0.0.0.0",
    port: 5173,
  },
  plugins: [react()],
});


```


**E. On lance le container :**

```bash
docker-compose up --build -d
```
- Explication du fichier docker-compose
    
    ```jsx
    version: "3"
    services:
      vite-react:
        image: node:18-alpine
        working_dir: /app
        ports:
          - "5173:5173"
        volumes:
          - ./my-app:/app
        command: ["sh", "-c", "npm install && npm run dev"]
    ```
    - Créée un container “vite-react” avec comme image : `image: node:18-alpine`
    - défini le dossier de travail dans le container : `working_dir: /app`
    - choisi un port pour l’app :
     `ports:
          - "5173:5173"`
    - On map nos fichiers en local (my-app) pour être chargé à la place des fichiers qui sont dans le dossier app du container. les `:` font office de séparateur *local*`:`*container*
    `volumes:
          - ./my-app:/app`
    - Une fois que le container est lancer on lui lance un `npm run dev` 
    `command: ["sh", "-c", "npm install && npm run dev"]`
    

**H. ready to code!** 

```bash
http://localhost:5173/

```

### Quelques commande Docker utiles

`docker-compose up --build -d` : lancer le container 1ère fois

`docker-compose up -d` : lancer le container au quotidien

`docker-compose down` : stopper le container

`docker-compose down -v` : stopper le container et supprimer les volumes


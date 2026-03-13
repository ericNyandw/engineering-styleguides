# 🐳 Mémo Docker : Partage & Commandes "Pro"

### 1. Partager ton projet
Deux stratégies pour le portfolio :

*   **Option A : Partage via GitHub (Le Code)**  
    L'utilisateur clone du dépôt [`fullstack-lab-codegen`](https://github.com/ericNyandw/fullstack-lab-codegen) et lance :
    ```bash
    docker-compose up --build
    ```
    *Docker reconstruit tout localement à partir de tes Dockerfiles.*

*   **Option B : Partage via Docker Hub (L'Image)**  
    Pour permettre de lancer le projet **sans le code source** :
    1.  **Login** : [`docker login`](https://login.docker.com/) sur ton terminal.
    2.  **Tag (Renommer)** :
        ```bash
        docker tag my-fullstack-lab/backend:1.0 ton-pseudo/fullstack-backend:1.0
        docker tag my-fullstack-lab/frontend:1.0 ton-pseudo/fullstack-frontend:1.0
        ```
    3.  **Push** :
        ```bash
        docker push ton-pseudo/fullstack-backend:1.0
        docker push ton-pseudo/fullstack-frontend:1.0
        ```
    *L'utilisateur n'a besoin que du `docker-compose.yml` (en pointant sur tes images) pour faire un `docker-compose up`.*

---

### 🛠️ 2. Manipulations indispensables au quotidien

*   **Pause Rapide** : `docker-compose stop`  
    *Arrête les services sans rien supprimer (le statut "Up 33 seconds" reprendra plus tard).*
*   **Nettoyage Standard** : `docker-compose down`  
    *Supprime les conteneurs et le réseau. Tes données sont conservées dans le volume.*
*   **Reset Total (Données incluses)** : `docker-compose down -v`  
    *⚠️ Vide ta base PostgreSQL (utile pour repartir de zéro).*
*   **Surveillance** : `docker-compose logs -f`  
    *Affiche les logs (Java/Front) en temps réel sans bloquer ton terminal.*
*   **Mode "Inside"** : `docker exec -it spring-backend sh`  
    *Entre dans le conteneur (comme en SSH) pour inspecter les fichiers internes.*

---

### ⚡ 3. Astuces "Gain de temps" (Build Ciblé)

Si tu modifies une seule partie du projet, ne relance pas tout le système :

*   **Modif Java** : `docker-compose up -d --build backend`
*   **Modif Angular** : `docker-compose up -d --build frontend`

*Docker recompile uniquement le service concerné, tes autres conteneurs (comme la DB) restent actifs.*

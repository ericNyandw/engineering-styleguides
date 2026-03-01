# 🐳 Docker Essentials: Le Guide Complet

Voici les commandes et paramètres essentiels pour maîtriser Docker, de la gestion de base aux astuces avancées.

---

## 🛠 1. La Logique de Commande (Règle d'Or)
Respectez toujours cet ordre de "poupée russe" pour éviter les bugs :
`docker [ACTION] [OPTIONS] [IMAGE] [COMMANDE_INTERNE]`

*   **ACTION** : Le verbe (`run`, `stop`, `rm`, `ps`, `build`, etc.).
*   **OPTIONS** : Les réglages/flags (`-d`, `-p`, `-v`, `--name`, `-e`).
*   **IMAGE** : Le nom de l'image. **Toujours à la fin des options !**
*   **COMMANDE_INTERNE** : (Optionnel) Ce qui est lancé dans le conteneur (ex : `bash`, `psql`).

> 💡 **Pourquoi ?** Tout ce qui est placé **après** le nom de l'image est ignoré par Docker et envoyé directement au processus interne.

---

## 🚀 2. Commandes essentielles au quotidien

### Gestion des Conteneurs
* **`docker ps`** : Liste les conteneurs actifs (`-a` pour tous, `-q` pour les IDs).
* **`docker run [image]`** : Crée et démarre un conteneur.
    * `-d` : Arrière-plan (detach).
    * `-p 8080:80` : Mapping Port (**HÔTE : CONTENEUR**).
    * `--name [nom]` : Donne un nom au conteneur.
    * `--rm` : Supprime le conteneur automatiquement à l'arrêt.
* **`docker stop [id/nom]`** : Arrête un conteneur.
* **`docker rm -f [id/nom]`** : Supprime un conteneur (force).

### Entrer et Agir (Debug & Flux)
* **`docker logs -f [nom]`** : (L'Espion 🕵️) Voir le flux sans risque. `Ctrl+C` ne coupe pas le conteneur.
* **`docker attach [nom]`** : (Le Patron ✋) Se coller au processus principal (PID 1). `Ctrl+C` **arrête** le conteneur.
* **`docker exec -it [nom] bash`** : (L'Invité 🚪) Ouvrir une session SSH secondaire pour inspecter.

### Gestion des Images
* **`docker images`** : Liste les images locales.
* **`docker pull [image]`** : Télécharge une image depuis le Hub.
* **`docker build -t [nom:tag] .`** : Construit une image à partir d'un `Dockerfile`.
* **`docker rmi [id/image]`** : Supprime une image.

---

## 🔥 3. Paramètres avancés et astuces "Power User"

### Variables d'environnement ET Filtres
* **`-e`** : Injecter une config : `docker run -e MYSQL_ROOT_PASSWORD=pass mysql`
* **`--env-file`** : Utiliser un fichier `.env`.
* **`--filter`** : Tri intelligent : `docker ps --filter "status=exited"`.
* **`--restart`** : Politique de relance (`always`, `unless-stopped`).

### Maintenance & Nettoyage
* **`docker stats`** : Consommation CPU/RAM en direct.
* **`docker system prune`** : Nettoyage global (radical).
* **Suppression massive** :
    * `docker stop $(docker ps -q)` : Stop tout.
    * `docker rm -f $(docker ps -aq)` : Supprime tout.

---

## 📦 4. Communication et Persistance

### Réseaux (Network Drivers). La Logique de l'Immeuble
* **`bridge`** : (Défaut) **Le Switch Virtuel**. Créez-en un **nommé** pour le DNS par nom.
* **`host`** : **Le Direct**. Le conteneur squatte les ports du PC. (Un seul habitant par port).
* **`none`** : **Le Bunker**. Isolation totale (pas de réseau).
* **Commandes** : `docker network create [nom]` / `docker network ls`.

### Persistance des données (Volumes)
**Règle d'or** : Ne jamais stocker de données importantes dans un conteneur.
* **Volume nommé (Production)** : `-v data_vol:/var/lib/mysql` (Géré par Docker).
* **Bind Mount (Développement)** : Lien direct vers un dossier local (`-v C:/MaBase:/data`).

---

## 🐧 5. Bonus : Mémo Arborescence Linux
* **`/`** : **La Racine**. Le début de tout.
* **`/home/user`** : Ton espace personnel.
*   **`/opt`** : Dossier recommandé pour les données persistantes et les apps tierces.
* **`./`** : Le dossier actuel (**PWD**).

---
*Guide validé et certifié - Mars 2026* 🚀

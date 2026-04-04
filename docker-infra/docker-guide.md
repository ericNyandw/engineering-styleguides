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

### Gestion des Images & Build
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

## 6. 🛠 Dépannage Flash (Builder & API)
* **`DOCKER_BUILDKIT=0`**: Désactive le moteur moderne (utile si le build plante sans raison).
* **`$env:DOCKER_API_VERSION="1.44`"** : Répare la connexion si ton terminal est "trop vieux" pour Docker.
* **`docker buildx ls`** : Vérifie si tes moteurs de build sont running ou inactive.
* **`docker buildx create --use --name [nom]`** : Crée un nouveau moteur si le default est cassé.

## 7. 📦 Réseaux & Problèmes (Network)
* **`docker network rm [nom_projet]_default`** : Supprime le réseau qui bloque (obligatoire si tu changes une option IPv4/IPv6).
* **Pourquoi `_default` ?** Docker Compose ajoute toujours `_default` au nom de ton dossier de projet par sécurité.
## 8. ⏸ Pause & Relance (Maintenance)
* **`docker pause [nom]`** : Gèle le conteneur (stoppe le CPU, garde la RAM).
* **`docker unpause [nom]`** : Réveille le conteneur là où il s'était arrêté.
* **`docker start [nom]`** : Relance un conteneur éteint (après un `stop` ou un redémarrage PC). **Zéro perte de données si tu as un Volume !**

* **⚠️ Alerte Fichier.env **
* **Syntaxe** : Utilise `=` (**`KEY=VAL`**). Jamais de : (**`KEY: VAL`**).
* **Warning** : Si tu vois `Python-dotenv could not parse`, c'est qu'une ligne est mal écrite.
---

**Astuce pour ton Docker Engine (JSON) :**
Si l'erreur `driver not connecting` revient, vérifie dans les réglages (JSON) que `"features": { "buildkit": true }` est bien présent. C'est l'interrupteur principal.

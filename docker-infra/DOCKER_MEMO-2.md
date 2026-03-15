# 📘 MÉMO : CONFIGURATION ET DÉPANNAGE TESTCONTAINERS (WINDOWS 11)

### Introduction
Ce guide explique comment établir une communication stable entre ton code **Java (Spring Boot)** et **Docker Desktop**. 
Le problème principal sur Windows 11 est que ces deux mondes ne se "voient" pas par défaut. 
Nous configurons un canal de communication via le **Port TCP 2375**.

---

## ÉTAPE 1 : Configurer le moteur (Docker Desktop)
C'est la base. Si Docker ne diffuse pas son signal, rien ne marchera.
1. Ouvrez **Docker Desktop**.
2. Cliquez sur l'icône de l'engrenage (**Settings**) en haut à droite.
3. Dans l'onglet **General**, cochez la case : `Expose daemon on tcp://localhost:2375 without TLS`.
4. Cliquez sur le bouton **Apply & Restart**.
    * **Précision** : Cela active l'écoute de Docker sur le **Port 2375**. C'est ce port précis que ton projet Java va "écouter" pour piloter les conteneurs.

---

## ÉTAPE 2 : Vérifier le canal (PowerShell)
Avant d'aller dans IntelliJ, vérifions si Windows accepte la connexion sur ce port.
1. Faites un clic droit sur le bouton **Démarrer** et choisissez **Terminal (Administrateur)**.
2. Tapez la commande suivante :
   `Test-NetConnection 127.0.0.1 -Port 2375`
3. **Résultat à vérifier** : Cherchez la ligne `TcpTestSucceeded`.
    * **Si elle affiche True** : Le port est ouvert et fonctionnel.
    * **Si elle affiche False** : Le port est bloqué.
        * **Action Pare-feu** : Forcez l'ouverture avec cette commande :
          `New-NetFirewallRule -DisplayName "Docker TCP 2375" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 2375`
        * **Action Antivirus** : Si le test échoue encore, désactivez temporairement votre antivirus. S'il passe à True, vous devez ajouter une exception pour le port 2375 dans les réglages de votre antivirus (ex: Bitdefender, Avast).

---

## ÉTAPE 3 : Configurer le GPS de Windows (Variables d'Environnement)
Il faut que Windows retienne l'adresse de Docker pour tous vos projets.
1. Recherche Windows > Tapez **"Variables d'environnement"** > **"Modifier les variables d'environnement système"**.
2. Bouton **"Variables d'environnement..."** (en bas à droite).
3. Section **"Variables utilisateur"** (en haut) > **Nouvelle** :
    * **Nom** : `DOCKER_HOST` / **Valeur** : `tcp://127.0.0.1:2375`
4. **Vérification (Crucial)** : Fermez et rouvrez PowerShell pour qu'il lise la modification, puis tapez :
   `Test-Path Env:\DOCKER_HOST`
    * Doit retourner **True**. (Pour voir la valeur, tapez : `echo $env:DOCKER_HOST`).

---

## ÉTAPE 4 : Le fichier de configuration global (.properties)
C'est le fichier qui "donne les ordres" à la bibliothèque Testcontainers sur ton PC.
1. Le fichier se trouve ici : `C:\Users\votre_nom\.testcontainers.properties`
2. **Commande PowerShell pour le créer proprement** :
```powershell
"docker.host=tcp://127.0.0.1:2375
testcontainers.reuse.enable=true
api.version=1.44
testcontainers.docker.client.strategy=org.testcontainers.dockerclient.EnvironmentAndSystemPropertyClientProviderStrategy" | Out-File -FilePath "$HOME\.testcontainers.properties" -Encoding ascii
```
**Pourquoi ces lignes ?**
- **reuse.enable=true :** Garde le container PostgreSQL allumé (gain de temps énorme).
- **api.version=1.44 :** Force le langage compatible avec Docker Desktop 4.64+.
- **strategy :** Force Testcontainers à regarder vos réglages TCP plutôt que de chercher les réglages Windows par défaut.

---

## ÉTAPE 5 : Vérifier la "Radio" (IntelliJ IDEA)
Dernière étape : dire à votre projet d'utiliser ce canal.

**A. Vérification de la connexion globale**
1. Allez dans **Settings > Build, Execution, Deployment > Docker**.
2. Cliquez sur le **+** et sélectionnez **TCP Socket**.
3. Entrez l'URL : `tcp://localhost:2375`.
4. Résultat :
   1. **Connection successful :** Tout est prêt !
   2. **Connection failed :** Reprenez l'Étape 1 et 2

**B. Configuration des modèles de tests (Templates)**
1. **Run > Edit Configurations... > Edit configuration templates... > JUnit**.
2. Dans le champ VM options, ajoutez :
   `-ea -Ddocker.host=tcp://127.0.0.1:2375 -Dapi.version=1.44`
3. Dans **Environment variables**, ajoutez : `DOCKER_HOST=tcp://127.0.0.1:2375`

---

## 🛠 RÉSUMÉ DES PANNES (LE DIAGNOSTIC RAPIDE)

| Ce que je vois (Erreur)                                                 |                                                                            Ce que je dois vérifier (Action)                                                                             |  
|:------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| **"Could not find a valid Docker environment"**                         |                                                   1.Docker Desktop est-il lancé ? <br/>2.La case du Port 2375 est-elle cochée ?<br/>                                                    |
| **"Status 400 (Bad Request)"**                                          |                                              La version d'API est-elle forcée ?<br/> -> Vérifier `-Dapi.version=1.44` dans IntelliJ.<br/>                                               |
| **"TcpTestSucceeded : False"**                                          |                                                  Le Pare-feu ou l'Antivirus bloque.<br/> -> Créer la règle `New-NetFirewallRule`.<br/>                                                  |
| **"Reuse was requested but not enabled"**                               |                                           Le fichier .testcontainers.properties est absent ou mal configuré.<br/>  -> Refaire l'Étape 4.<br/>                                           |

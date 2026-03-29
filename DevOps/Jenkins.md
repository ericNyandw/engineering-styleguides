#  🎯 MODULE 1 : INSTALLATION & CONNEXION GITHUB (Windows 11)

Bienvenue dans la formation DevOps ! Je vais te guider pas à pas. Chaque étape sera expliquée en détail

---

**📦 PARTIE 1.1 : INSTALLATION DE JENKINS SUR WINDOWS 11**

## Étape 1 : Téléchargement de Jenkins
1. **Ouvre ton navigateur** et va sur : https://www.jenkins.io/download/
2. **Télécharge la version Windows** (fichier .msi ou .war)
*   **Je te recommande la version**.`war`pour mieux comprendre le fonctionnement interne

**Pourquoi le .war ?**

*   C'est un fichier Java exécutable
*   Tu verras clairement comment Jenkins démarre
*   Plus pédagogique pour un débutant

## Étape 2 : Vérification des prérequis
Ouvre un **PowerShell ou CMD** et vérifie :

```bash
java -version
```
Résultat attendu :
```text
java version "17.x.x" ou supérieur
```
✅ Si OK, continue
❌ Si erreur : vérifie que la variable d'environnement JAVA_HOME pointe vers ton JDK 17+

---
##  Étape 3 : Lancement de Jenkins
1. **Crée un dossier dédié** (exemple : C :\Jenkins)
2. **Déplace le fichier** jenkins.war dans ce dossier
3. **Ouvre PowerShell dans ce dossier** (Shift + Clic droit → "Ouvrir PowerShell ici")
4. **Lance Jenkins avec cette commande :**

```bash
java -jar jenkins.war --httpPort=8080
```
📌 **Explication de la commande :**

*  java-jar : exécute le fichier WAR comme une application Java
*  jenkins.war : le fichier téléchargé
*  --httpPort=8080 : Jenkins sera accessible sur http://localhost:8080

**Ce que tu vas voir :**

*  Beaucoup de logs qui défilent (c'est normal !)
*  Un message important contenant un mot de passe administrateur

---
## Étape 4 : Premier accès à Jenkins
1. **Copie le mot de passe** qui apparaît dans les logs (ressemble à : a1b2c3d4e5f6...)

*  Il est aussi stocké dans : C:\Users\TonNom\.jenkins\secrets\initialAdminPassword
1. **Ouvre ton navigateur** et va sur : http://localhost:8080

2. **Colle le mot de passe** dans la page "Unlock Jenkins"

3. **Choisis "Install suggested plugins"**

*  Jenkins va installer les plugins de base (Git, Maven, etc.)
*  ⏱ Patience : ça prend 3-5 minutes

1. **Crée ton compte administrateur**
*   Username : admin (ou ton choix)
*   Mot de passe : choisis-en un fort
*   Email : ton email

---

## 🔧 PARTIE 1.2 : CONFIGURATION DES OUTILS (JDK & MAVEN)
Jenkins doit savoir où trouver ton JDK et Maven.

## Étape 5 : Configuration du JDK
1. Dans Jenkins, clique sur **"Manage Jenkins"** (dans le menu de gauche).
2. Clique sur **"Global Tool Configuration"**
3. Descends jusqu'à **"JDK"**
4. Clique sur **"Add JDK"**
*   **Name :** JDK17 (nom que tu utiliseras dans ton Jenkinsfile)
*   **Décoche** "Install automatically"
*   **JAVA_HOME :** indique le chemin de ton JDK (exemple : C :\Program Files\Java\jdk-17)

💡 **Comment trouver ton JAVA_HOME ?**

```bash
echo %JAVA_HOME%
```
Ou vérifie dans tes variables d'environnement Windows.
---
## Étape 6 : Configuration de Maven
1. **Toujours dans "Global Tool Configuration"**
2. Descends jusqu'à **"Maven"**
3. Clique sur **"Add Maven"**
*   **Name :** Maven3.9 (nom pour le Jenkinsfile)
*   **Décoche "** Install automatically"
*   **MAVEN_HOME :** indique le chemin de ton Maven (exemple : C :\Maven\ apache maven-3.9.x)
💡 Comment trouver ton Maven ?

```bash
mvn -version
```
Le chemin s'affiche (exemple : Maven home: C:\Maven\...)

4. **Clique sur "Save"** (en bas de la page)
---

## Étape 7 : Génération d'un Personal Access Token (PAT) GitHub
Jenkins a besoin d'un "mot de passe" pour accéder à ton dépôt privé.

1. Va sur GitHub → Clique sur ta photo de profil (en haut à droite)
2. Settings → Developer settings (tout en bas à gauche)
3. Personal access tokens → Tokens (classic)
4. Generate new token (classic)
5. Configure le token :
*   **Note** : Jenkins CI/CD
*   **Expiration** : 90 days (ou No expiration pour tester)
*   **Coche ces permissions** :
  *   ✅ repo (tous les sous-éléments)
  *   ✅ admin:repo_hook (pour les webhooks plus tard)
6. **Clique sur "Generate token"**
7. ⚠️ **COPIE LE TOKEN IMMÉDIATEMENT** (tu ne pourras plus le revoir !)

---

## Étape 8 : Ajout du token dans Jenkins
1. **Retourne dans Jenkins → Manage Jenkins → Manage Credentials**
2. Clique sur **"(global)"** sous "Stores scoped to Jenkins"
3. Clique sur **"Add Credentials"** (à gauche)
4. **Remplis le formulaire :**
*   **Kind :** `Username with password`
*   **Username :** ton username GitHub (exemple : `john-dev`)
*   **Password :** colle le **token GitHub** (pas ton mot de passe GitHub !)
*   **ID :** `github-token` (identifiant unique pour le Jenkinsfile)
*   **Description :** `GitHub Access Token`
5. **Clique sur "Create"**

---

## Étape 9 : Création de ton premier Pipeline Jenkins
1. Retourne à la page d'accueil de Jenkins
2. Clique sur **"New Item"** (en haut à gauche)
3. Configure le projet :

*   **Nom** :  `spring-boot-cicd ` (ou le nom de ton projet)
*   **Type** : sélectionne "Pipeline"
*   **Clique sur "OK"**
4. **Dans la page de configuration qui s'ouvre :**

*   **Description** :  `Mon premier pipeline CI/CD pour Spring Boot `
*   Descends jusqu'à **"Pipeline"**

**📌 Pourquoi "from SCM" ?**

*   SCM = Source Code Management (Git)
*   Le Jenkinsfile sera stocké **dans ton projet GitHub**, pas dans Jenkins
*   Meilleure pratique professionnelle !

5.Configure la connexion Git :

*   **SCM** : Git
*   **Repository URL** : colle l'URL de ton repo (exemple : https://github.com/ton-username/ton-projet.git)
*   **Credentials** : sélectionne github-token (créé à l'étape 8)
*   **Branch Specifier** : */main (ou */master selon ton repo)
*   **Script Path** : Jenkinsfile (nom du fichier que tu créeras dans ton repo)
6. **Clique sur "Save"**
---
✅ **VALIDATION DU MODULE 1**
**Avant de passer au Module 2, vérifie que :**

1. ✅ Jenkins tourne sur http://localhost:8080
2. ✅ JDK et Maven sont configurés dans Jenkins
3. ✅ Ton token GitHub est enregistré dans Jenkins
4. ✅ Ton pipeline spring-boot-cicd est créé et connecté à GitHub
---
🎯 **ACTION IMMÉDIATE**
**Exécute toutes ces étapes et réponds-moi :**

1. **Capture d'écran** (ou description) de ta page d'accueil Jenkins
2. **Confirme** que ton pipeline apparaît dans la liste
3. **Dis-moi** si tu rencontres des erreurs à une étape précise

**Dès que tu réponds "OK", je te donnerai le Module 2 avec ton premier Jenkinsfile !** 🚀

## 🎯 Décryptage de l'interface Jenkins
## 📊 COMPRENDRE L'INTERFACE JENKINS

**Ce que signifient les colonnes :**

| **Colonne**       | **Signification**            |              **Dans ton cas**              |  
|:------------------|:-----------------------------|:------------------------------------------:|
| **Last Success**  | Date du dernier build réuss  | **N/A** = Aucun build n'a encore été lancé |
| **Last Failure**  | Date du dernier build échoué |  **N/A** = Aucun build n'a encore échoué   |
| **Last Duration** | Durée du dernier build <br/> |  **N/A** = Aucun build n'a encore tourné   |


🔍 **Conclusion :** Ton pipeline existe, mais **il n'a jamais été exécuté.**
---

## 🚀 COMMENT LANCER UN BUILD ET VOIR LES ERREURS ?
## Méthode 1 : Build manuel (pour tester)
## Étape 1 : Lancer le build
1. Clique sur le nom de ton pipeline : `java-products-lab`
2. Dans le menu de gauche, **clique sur "Build Now"**
**Ce qui va se passer :**

Jenkins va aller chercher ton code sur GitHub
Il va chercher un fichier `Jenkinsfile` à la racine de ton projet
⚠️ **ERREUR ATTENDUE** : Tu n'as pas encore cr
---

## Étape 2 : Voir le résultat du build
Après avoir cliqué sur "Build Now", tu verras apparaître :

1. **Dans "Build History"** (en bas à gauche) : Un numéro **#1** avec une icône

*   🔴 **Boule rouge** = Échec (erreur)
*   🔵 **Boule bleue** = Succès
*   ⚪ **Boule grise** = En cours ou jamais exécuté
2. **Clique sur le numéro #1**

3. **Clique sur "Console Output"** (menu de gauche)
---

## Étape 3 : Lire les logs (Console Output)
Tu verras quelque chose comme ça :

```{}text
Started by user admin
Checking out git https://github.com/ton-username/java-products-lab.git
...
ERROR: Couldn't find any revision to build. Verify the repository and branch configuration for this job.
```
🔍 **Traduction de l'erreur :**

*   Jenkins a réussi à se connecter à GitHub ✅
*   Mais il ne trouve **pas de fichier Jenkinsfile** ❌
**C'est NORMAL ! On va le créer maintenant ensemble.**

---

## 📝 CRÉATION DE TON PREMIER JENKINSFILE (Version Minimale)

## Étape 4 : Créer le Jenkinsfile dans ton projet
1. **Ouvre ton projet Spring Boot dans IntelliJ**

2. À la racine du projet (au même niveau que pom.xml), crée un nouveau fichier :

*   Nom : Jenkinsfile (sans extension !)
*   Type : Fichier texte simple
3. **Colle ce code minimal** (je vais expliquer chaque ligne) :

```{}groovy
pipeline {
    agent any
    
    tools {
        jdk 'JDK17'
        maven 'Maven3.9'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📥 Récupération du code depuis GitHub...'
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Compilation du projet avec Maven...'
                bat 'mvn clean package -DskipTests'
            }
        }
    }
}
```

## 📖 EXPLICATION LIGNE PAR LIGNE (Cours de Groovy pour débutant)
**Bloc 1 : La structure** `pipeline`

```{}groovy
pipeline {
    // Tout le code du pipeline va ici
}
```
*   C'est le conteneur principal de ton pipeline
*   Obligatoire : Tout Jenkinsfile commence par ça
*   Groovy syntax : Les accolades {} délimitent un bloc de code

---

**Bloc 2 **: `agent any`
```{}groovy
agent any
```
*   **Agent** = La machine qui va exécuter ton pipeline
*   `any` = Jenkins choisit n'importe quel agent disponible
*   Dans ton cas : C'est ta machine Windows (le "master" Jenkins)
💡 **Alternative (que tu verras plus tard) :**

```{}groovy
agent {
    docker { image 'maven:3.9-jdk-17' }  // Utiliser un conteneur Docker
}
```
**Bloc 3 :** `tools`
```{}groovy
tools {
    jdk 'JDK17'
    maven 'Maven3.9'
}
```
*   **Déclare les outils nécessaires** pour le build
*   `'JDK17'` et `'Maven3.9'` : Ce sont les **noms exacts** que tu as donnés dans "Global Tool Configuration" (Module 1, Partie 1.2)
*   Jenkins va automatiquement utiliser ces outils pour toutes les étapes
⚠️ **SI TU AS UTILISÉ D'AUTRES NOMS :**

*   Retourne dans Jenkins → Manage Jenkins → Global Tool Configuration
*   Note les noms exacts et modifie le Jenkinsfile
---

**Bloc 4 :** `stages`
```{}groovy
stages {
    stage('Checkout') { ... }
    stage('Build') { ... }
}
```
*   `stages` = Ensemble des étapes de ton pipeline
*   Chaque `stage` = Une phase distincte (checkout, build, test, deploy...)
*   **Bonne pratique** : Donner des noms clairs aux stages

---

**Stage 1 : Checkout**

```{}groovy
stage('Checkout') {
    steps {
        echo '📥 Récupération du code depuis GitHub...'
        checkout scm
    }
}
```
*   `steps` = Les actions à exécuter dans ce stage
*   `echo` = Affiche un message dans la console (comme System.out.println en Java)
*   `checkout scm` = Commande Jenkins pour récupérer le code depuis Git
  *   `scm` = Source Code Management (la config GitHub que tu as faite dans Jenkins)

---

**Stage 2 : Build**

```{}groovy
stage('Build') {
    steps {
        echo '🔨 Compilation du projet avec Maven...'
        bat 'mvn clean package -DskipTests'
    }
}
```

*   `bat` = Exécute une commande Windows (Batch)
  *   Sur Linux/Mac, on utilise sh à la place
*   `mvn clean package` = Commande Maven que tu connais déjà
  *   `clean` : Supprime le dossier `target/`
  *   `package` : Compile et crée le fichier JAR
  *   `-DskipTests` : Ignore les tests pour l'instant (on les ajoutera plus tard)

---

## 🚀 PASSER À L'ACTION
## Étape 5 : Commiter le Jenkinsfile sur GitHub
Dans **IntelliJ** :

1. **Ouvre le terminal Git** (en bas)
2. **Exécute ces commandes** :

```{}Bash
git add Jenkinsfile
git commit -m "feat: Add basic Jenkinsfile for CI/CD"
git push origin main
```
(Remplace main par master si c'est ton nom de branche)

---

## Étape 6 : Lancer le build dans Jenkins
1. **Retourne sur Jenkins →** Ton pipeline java-products-lab
2. **Clique sur "Build Now"**
3. **Observe "Build History" →** Un nouveau **#1** (ou #2) apparaît
4. **Clique sur le numéro → "Console Output"**

---
🎯 **CE QUE TU DOIS VOIR**

**Dans la Console Output :**
```{}text
Started by user admin
...
[Pipeline] stage (Checkout)
📥 Récupération du code depuis GitHub...
Cloning repository https://github.com/...
...
[Pipeline] stage (Build)
🔨 Compilation du projet avec Maven...
[INFO] BUILD SUCCESS
...
Finished: SUCCESS
```
**Dans la liste des pipelines :**

*   **Last Success** : Maintenant avec une date/heure ✅
*   **Last Duration** : ~1-3 minutes (selon ton projet)
*   **Icône** : 🔵 Boule bleue (succès) ou 🌤️ jaune (instable)

---

❌ **SI TU AS UNE ERREUR**
**Erreur 1 : "bat: command not found"**
**Solution :** Remplace `bat` par `sh` (tu es peut-être sur Git Bash)

**Erreur 2 : "Tool type 'jdk' does not have an install of 'JDK17'"**
**Solution :** Le nom du JDK ne correspond pas. Vérifie dans Global Tool Configuration.

**Erreur 3 : Maven échoue avec un message de dépendances**
**Solution :** Ton `pom.xml` a peut-être un problème. Copie-moi l'erreur exacte.

---

✅ **VALIDATION**
**Réponds-moi avec :**

1. ✅ **Statut du build** : Succès (🔵) ou Échec (🔴) ?
2. 📸 **Copie les 20 dernières lignes** de la Console Output
3. ❓ **Décris toute erreur** si le build échoue
**Dès que ton build est vert (SUCCESS), on passe au Module 2 complet avec :**

*   Archivage des artefacts (conservation du JAR)
*   Gestion des erreurs avancée
*   Structuration professionnelle du Jenkinsfile
**Prêt à tester ? GO ! 🚀**

IF OK
**🎉 PARFAIT ! TON PREMIER BUILD EST UN SUCCÈS !**
✅ **ANALYSE DE TON BUILD RÉUSSI**

Regardons ce qui s'est passé :

```{}text
[INFO] Building jar: C:\Users\ericn\.jenkins\workspace\java-products-lab\target\java-products-lab-0.0.1-SNAPSHOT.jar
[INFO] --- spring-boot-maven-plugin:3.4.5:repackage (repackage) @ java-products-lab ---
[INFO] BUILD SUCCESS
[INFO] Total time: 29.359 s
```
🎯 **Ce qui a fonctionné :**
✅ **Checkout** : Code récupéré depuis GitHub
✅ **Compilation** : 9 fichiers Java compilés
✅ **Tests ignorés** : –DskipTests a fonctionné
✅ **JAR créé** : java-products-lab-0.0.1-SNAPSHOT.jar
✅ **Spring Boot repackage** : JAR exécutable créé
✅ **Durée** : 29 secondes (normal pour un premier build)

---
📦 **TON ARTEFACT (JAR) EST MAINTENANT DISPONIBLE**
**Emplacement du fichier :**

```{}text
C:\Users\ericn\.jenkins\workspace\java-products-lab\target\java-products-lab-0.0.1-SNAPSHOT.jar
```
⚠️ **PROBLÈME** : Si tu relances un build, ce fichier sera **écrasé** !

## 🚀 MODULE 2 : AMÉLIORATION DU JENKINSFILE (Architecture Professionnelle)

Maintenant qu'on a un build fonctionnel, on va améliorer le pipeline avec :

1. **Archivage des artefacts** (conserver le JAR dans Jenkins)
2. **Gestion des erreurs** (cleanup automatique)
3. **Notifications** (savoir quand ça échoue)
4. **Structure professionnelle** (post-actions)
5. 
---

## 📝 NOUVEAU JENKINSFILE (Version Professionnelle)
## Étape 1 : Mise à jour du code
**Dans IntelliJ**, ouvre ton Jenkinsfile et remplace TOUT le contenu par :
```{}Bash
pipeline {
    agent any
    
    tools {
        jdk 'JDK17'
        maven 'Maven3.9'
    }
    
    environment {
        // Variables d'environnement globales
        APP_NAME = 'java-products-lab'
        BUILD_VERSION = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '================================================'
                echo 'ETAPE 1 : Recuperation du code source'
                echo '================================================'
                checkout scm
                echo 'Code recupere avec succes depuis GitHub'
            }
        }
        
        stage('Build') {
            steps {
                echo '================================================'
                echo 'ETAPE 2 : Compilation et Packaging'
                echo '================================================'
                bat 'mvn clean package -DskipTests'
                echo 'Build termine avec succes'
            }
        }
        
        stage('Archive') {
            steps {
                echo '================================================'
                echo 'ETAPE 3 : Archivage des artefacts'
                echo '================================================'
                archiveArtifacts artifacts: 'target/*.jar', 
                                 fingerprint: true,
                                 allowEmptyArchive: false
                echo 'Artefacts archives dans Jenkins'
            }
        }
    }
    
    post {
        success {
            echo '================================================'
            echo 'PIPELINE REUSSI !'
            echo "Build #${env.BUILD_NUMBER} termine avec succes"
            echo "Artefact disponible : ${APP_NAME}-${BUILD_VERSION}.jar"
            echo '================================================'
        }
        
        failure {
            echo '================================================'
            echo 'PIPELINE ECHOUE !'
            echo "Build #${env.BUILD_NUMBER} a echoue"
            echo 'Consultez les logs pour plus de details'
            echo '================================================'
        }
        
        always {
            echo '================================================'
            echo 'NETTOYAGE DU WORKSPACE'
            echo '================================================'
            cleanWs()
        }
    }
}
```

---

📖 EXPLICATION DÉTAILLÉE (Cours Groovy - Niveau 2)
🔹 **Bloc 1 :** environment

```{}groovy
environment {
    APP_NAME = 'java-products-lab'
    BUILD_VERSION = "${env.BUILD_NUMBER}"
}
```
**Qu'est-ce que c'est ?**

*   Définit des variables globales utilisables dans tout le pipeline
*   APP_NAME : Nom de ton application (string statique)
*   BUILD_VERSION : Numéro du build actuel (dynamique)

**Syntaxe Groovy** :

*   "${env.BUILD_NUMBER}" : Interpolation de chaîne (comme ${} en JavaScript)
*   env.BUILD_NUMBER : Variable Jenkins automatique (1, 2, 3...)

**Utilisation :**
```{}groovy
echo "Mon app : ${APP_NAME}"  // Affiche : Mon app : java-products-lab
echo "Version : ${BUILD_VERSION}"  // Affiche : Version : 5
```
---
🔹 **Bloc 2 : Stage** `Archive`

```{}groovy
stage('Archive') {
    steps {
        archiveArtifacts artifacts: 'target/*.jar', 
                         fingerprint: true,
                         allowEmptyArchive: false
    }
}
```
**Fonction** `archiveArtifacts` :

| **Paramètre**       | **Signification**      |                      **Valeur**                      |  
|:--------------------|:-----------------------|:----------------------------------------------------:|
| `artifacts`         | Fichiers à archiver    |  `'target/*.jar'` = Tous les JAR du dossier target   |
| `fingerprint`       | Traçabilité (hash MD5) |     `true` = Jenkins peut comparer les versions      |
| `allowEmptyArchive` | Autoriser zéro fichier |         `false` = Erreur si aucun JAR trouvé         |

**Syntaxe Groovy :**

*  Pattern `*.jar` : Wildcard (tous les fichiers `.jar`)
*  Chemin relatif : Depuis la racine du workspace Jenkins

**Résultat :**
*  Le JAR sera **copié dans Jenkins** (en dehors du workspace)
*  Accessible via l'interface web : **"Build #X" → "Build Artifacts"**
*  Conservé même après un cleanWs()

---

🔹 **Bloc 3 :** `post`
```{}groovy
post {
    success { ... }
    failure { ... }
    always { ... }
}
```
**Qu'est-ce que** `post` ?

*   Bloc exécuté **APRÈS** tous les stages
*   Contient des **conditions** selon le résultat du build

**Les conditions disponibles :**


| **Condition**     |                **Quand s'exécute-t-elle ?**                |  
|:------------------|:----------------------------------------------------------:|
| `success`         |                 Build réussi (vert)                        | 
| `failure`         |                    Build échoué (rouge)                    |     
| `unstable`        | Build instable (jaune - tests échoués mais compilation OK) |        
| `always`          |            Toujours, quel que soit le résultat             |        
| `changed`         |     Le statut a changé (succès → échec ou inversement)     |        

**Exemple pratique :**

```{}groovy
post {
    success {
        // Envoyer un email de succès
        // Déployer en production
    }
    failure {
        // Envoyer une alerte Slack
        // Rollback automatique
    }
    always {
        // Nettoyage des ressources
        // Archivage des logs
    }
}
```
---

🔹 **Fonction  :** `cleanWs()`

```{}groovy
cleanWs()
```
**Qu'est-ce que ça fait ?**

*   Clean Workspace = Supprime TOUS les fichiers du workspace
*   Libère de l'espace disque
*   Garantit un build "propre" au prochain lancement

**Workspace path :**

```{}text
C:\Users\ericn\.jenkins\workspace\java-products-lab\
```
⚠️ **Pourquoi c'est sûr ?**

*   Les artefacts archivés sont **hors workspace** (dans `C:\Users\ericn\.jenkins\jobs\java-products-lab\builds\X\archive\`)
*   Le code source sera re-cloné au prochain build

---
**🚀 MISE EN PRATIQUE**

## Étape 2 : Commit et Push

```{}Bash
git add Jenkinsfile
git commit -m "feat: Add artifact archiving and post-actions"
git push origin main
```
---

## Étape 3 : Lancer le nouveau build
1. **Jenkins** → Ton pipeline → **Build Now**
2. **Clique sur le numéro du build**
3. **Observe la Console Output**

---

## Étape 4 : Vérifier les artefacts archivés

**Dans Jenkins :**

1. **Clique sur ton build** (ex: #5)
2. **Tu devrais voir une nouvelle section : "Build Artifacts"**
3. **Clique sur le fichier JAR** → Il se télécharge !

**🎯 TEST :**

*   Lance 2 builds (Build Now × 2)
*   Vérifie que **chaque build conserve son JAR**
*   Build #5 et Build #6 ont chacun leur artefact

---

📊 CE QUE TU DOIS VOIR
Console Output (extraits) :
```{}Bash
[Pipeline] stage (Checkout)
================================================
ETAPE 1 : Recuperation du code source
================================================
Code recupere avec succes depuis GitHub

[Pipeline] stage (Build)
================================================
ETAPE 2 : Compilation et Packaging
================================================
[INFO] BUILD SUCCESS
Build termine avec succes

[Pipeline] stage (Archive)
================================================
ETAPE 3 : Archivage des artefacts
================================================
Archiving artifacts
[Pipeline] archiveArtifacts
Archiving artifacts
[Pipeline] echo
Artefacts archives dans Jenkins

[Pipeline] stage (Declarative: Post Actions)
================================================
PIPELINE REUSSI !
Build #5 termine avec succes
Artefact disponible : java-products-lab-5.jar
================================================

[Pipeline] cleanWs
Deleting workspace...
Finished: SUCCESS
```

---

✅ VALIDATION DU MODULE 2

**Réponds-moi avec :**

1. ✅ Statut du build : SUCCESS ou FAILURE ?
2. 📦 Capture d'écran (ou description) de la section "Build Artifacts"
3. 🔢 Combien de builds as-tu lancés au total ?
4. ❓ Questions sur le code Groovy ?


**Prochaine étape (Module 3) :**

*   Intégration de **SonarQube** pour l'analyse de qualité
*   Configuration du **Quality Gate**
*   Le build échouera si la qualité est insuffisante

---

## 🎯 OBJECTIFS DU MODULE 3

Tu vas apprendre à :

1. Installer et configurer **SonarQube** en local (Windows)
2. Intégrer l'analyse dans ton **Jenkinsfile**
3. Configurer un **Quality Gate** (seuil de qualité)
4. Faire **échouer le build** si la qualité est insuffisante

---

**📦 PARTIE 3.1 : INSTALLATION DE SONARQUBE (Local Windows)**

## Étape 1 : Téléchargement de SonarQube

1. **Va sur** : https://www.sonarsource.com/products/sonarqube/downloads/
2. Télécharge : Community Edition (gratuite)
  *  Fichier : sonarqube-X.X.X.zip (environ 300 MB)
3. **Extrait le ZIP** dans un dossier simple :

```{}text
C:\SonarQube\
```
📁 **Structure attendue :**

```{}text
C:\SonarQube\
├── bin\
│   ├── windows-x86-64\
│   │   └── StartSonar.bat   ← Fichier de démarrage
├── conf\
├── data\
├── logs\
└── web\
```

---

## Étape 2 : Démarrage de SonarQube
1. Ouvre PowerShell en tant qu'administrateur
2. Navigue vers le dossier :

```{}Bash
cd C:\SonarQube\bin\windows-x86-64
```
3. **Lance SonarQube :**

```{}Bash
.\StartSonar.bat
```
📌 **Ce qui va se passer :**

*   Beaucoup de logs vont défiler
*   ⏱ **Attends 2-3 minutes** (première fois = initialisation de la base de données)
*   Cherche ce message :
```{}Text
SonarQube is operational
```
⚠️ Si tu vois une erreur "Java not found" :
```{}Bash
# Vérifie ta version Java
java -version

# Si besoin, définis JAVA_HOME
setx JAVA_HOME "C:\Program Files\Java\jdk-17"
```
---
## Étape 3 : Premier accès à SonarQube
1. **Ouvre ton navigateur** : http://localhost:9000
2. **Connexion par défaut :**
   *    Login : admin
   *    Password : admin
3. Change le mot de passe (obligatoire au premier accès)
   *    Nouveau mot de passe : choisis-en un (ex: Admin123!)
   *    Note-le bien (tu en auras besoin dans Jenkins)
   
**🎉 SonarQube est maintenant opérationnel !**
---

**🔧 PARTIE 3.2 : CONFIGURATION DE SONARQUBE**

## Étape 4 : Création d'un projet dans SonarQube

1. **Dans SonarQube**, clique sur **"Create Project"**
2. **Choose "Manually"**
3. **Rem plis le formulaire :**
    *    **Project display name :** `Java Products Lab`
    *    **Project key :** `java-products-lab` (identifiant unique, sans espaces)
    *    **Main branch name :** `main`
4. **Clique sur "Next"**
5. **Choose "Use the global setting"**(analyse continue)
6. **Clique sur "Create project"**

---

## Étape 5 : Génération du Token d'authentification
SonarQube va te demander **comment tu veux analyser :**

1. **Choisis : "With Jenkins"**
2. **Configure with Jenkins → Other (for JS, TS, Go, Python, PHP, ...)**
   *    (Maven est supporté mais on va utiliser le Scanner générique pour plus de flexibilité)
3. **Generate a token :**
   *    **Token name** : `jenkins-scanner`
   *    **Token type** : `Global Analysis Token`
   *    **Clique sur "Generate"**
⚠️ COPIE LE TOKEN (exemple : `squ_a1b2c3d4e5f6...`)
   *    Tu ne pourras plus le revoir !
📝 **Garde ce token sous la main** (on va le mettre dans Jenkins)

---

## Étape 7 : Configuration du serveur SonarQube dans Jenkins

1. **Dans Jenkins → Manage Jenkins → Configure System**
2. **Descends jusqu'à "SonarQube servers"**
3. **Coche**: `Enable injection of SonarQube server configuration`
4. **Clique sur "Add SonarQube"**
5. **Remplis :**
   *    **Name** : `SonarQube-Local` (nom que tu utiliseras dans le Jenkinsfile)
   *    **Server URL** : `http://localhost:9000`
   *    **Server authentication token** : Sélectionne `sonarqube-token`
6. **Clique sur "Save"**

---

## Étape 8 : Installation du plugin SonarQube Scanner
1. **Jenkins → Manage Jenkins → Manage Plugins**
2. **Onglet "Available plugins"**
3. **Recherche** : SonarQube Scanner
4. **Coche la case** et clique sur **"Install without restart"**
5. **Attends la fin de l'installation** (barre verte à 100%)

---

## Étape 9 : Configuration de l'outil SonarQube Scanner
1. **Jenkins → Manage Jenkins → Global Tool Configuration**
2. **Descends jusqu'à "SonarQube Scanner"**
3. **Clique sur "Add SonarQube Scanner"**
4. **Remplis :**
    *    **Name** : `SonarScanner` (nom pour le Jenkinsfile)
    *    **Coche** : `Install automatically`
    *    **Version** : Choisis la dernière version stable (ex: `SonarQube Scanner 6.2.1.4610`)
5. **Clique sur "Save"**

---

**📝 PARTIE 3.3 : MISE À JOUR DU JENKINSFILE**
## Étape 10 : Ajout du stage SonarQube
**Dans IntelliJ**, ouvre ton Jenkinsfile et **remplace** par cette version :

```{}groovy
pipeline {
    agent any
    
    tools {
        jdk 'JDK17'
        maven 'Maven3.9'
    }
    
    environment {
        APP_NAME = 'java-products-lab'
        BUILD_VERSION = "${env.BUILD_NUMBER}"
        SONAR_PROJECT_KEY = 'java-products-lab'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '================================================'
                echo 'ETAPE 1 : Recuperation du code source'
                echo '================================================'
                checkout scm
                echo 'Code recupere avec succes depuis GitHub'
            }
        }
        
        stage('Build') {
            steps {
                echo '================================================'
                echo 'ETAPE 2 : Compilation et Packaging'
                echo '================================================'
                bat 'mvn clean package -DskipTests'
                echo 'Build termine avec succes'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo '================================================'
                echo 'ETAPE 3 : Analyse de la qualite du code'
                echo '================================================'
                withSonarQubeEnv('SonarQube-Local') {
                    bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                        -Dsonar.projectName="${APP_NAME}" ^
                        -Dsonar.projectVersion=${BUILD_VERSION}
                    """
                }
                echo 'Analyse SonarQube terminee'
            }
        }
        
        stage('Quality Gate') {
            steps {
                echo '================================================'
                echo 'ETAPE 4 : Verification du Quality Gate'
                echo '================================================'
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
                echo 'Quality Gate passe avec succes'
            }
        }
        
        stage('Archive') {
            steps {
                echo '================================================'
                echo 'ETAPE 5 : Archivage des artefacts'
                echo '================================================'
                archiveArtifacts artifacts: 'target/*.jar', 
                                 fingerprint: true,
                                 allowEmptyArchive: false
                echo 'Artefacts archives dans Jenkins'
            }
        }
    }
    
    post {
        success {
            echo '================================================'
            echo 'PIPELINE REUSSI !'
            echo "Build #${env.BUILD_NUMBER} termine avec succes"
            echo "Qualite du code : VALIDE"
            echo '================================================'
        }
        
        failure {
            echo '================================================'
            echo 'PIPELINE ECHOUE !'
            echo "Build #${env.BUILD_NUMBER} a echoue"
            echo 'Cause possible : Quality Gate non respecte'
            echo '================================================'
        }
        
        always {
            echo '================================================'
            echo 'NETTOYAGE DU WORKSPACE'
            echo '================================================'
            cleanWs()
        }
    }
}
```

---

📖 **EXPLICATION DÉTAILLÉE (Cours Groovy - Niveau 3)**
🔹 **Stage** `SonarQube Analysis`
```{}groovy
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube-Local') {
            bat """
                mvn sonar:sonar ^
                -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                -Dsonar.projectName="${APP_NAME}" ^
                -Dsonar.projectVersion=${BUILD_VERSION}
            """
        }
    }
}
```

**Fonction** `withSonarQubeEnv` :

*    **Injecte automatiquement** les variables d'environnement SonarQube
*    `'SonarQube-Local'`  : Nom du serveur configuré dans Jenkins (Étape 7)
*    Établit la connexion avec http://localhost:9000

**Syntaxe Groovy - Multi-lignes :**

```{}groovy
bat """
    commande ligne 1 ^
    commande ligne 2 ^
    commande ligne 3
"""
```
*    `"""` : Triple quotes = string multi-lignes
*    `^` : Caractère de continuation Windows (équivalent de `\` sur Linux)

**Paramètres Maven SonarQube :**

| **Paramètre**            |                      **Signification**                       |  
|:-------------------------|:------------------------------------------------------------:|
| `sonar:sonar`            |               Goal Maven pour lancer l'analyse               | 
| `-Dsonar.projectKey`     | Identifiant unique du projet (doit correspondre à SonarQube) |     
| `-Dsonar.projectName`    |                 Nom affiché dans SonarQube                   |        
| `-Dsonar.projectVersion` |          Numéro de version (ici = numéro de build)           |        
    

---

🔹 **Stage** `Quality Gate`

```{}groovy
stage('Quality Gate') {
    steps {
        timeout(time: 5, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
```
**Fonction** timeout :

*    **Limite le temps d'attente à 5 minutes
*    Si SonarQube ne répond pas → Erreur

**Fonction** `waitForQualityGate` :

*    **Attend** que SonarQube termine l'analyse
*    **Vérifie** si le Quality Gate est "PASSED" ou "FAILED"
*    `abortPipeline: true` : Si FAILED → Le pipeline s'arrête immédiatement

**💡 Quality Gate = Seuil de qualité**
Exemples de règles :

*    Couverture de tests > 80%
*    Aucun bug bloquant
*    Aucune faille de sécurité critique
*    Duplication de code < 3%

---

**🚀 MISE EN PRATIQUE**

## Étape 11 : Commit et Push
```{}Bash
git add Jenkinsfile
git commit -m "feat: Add SonarQube analysis and Quality Gate"
git push origin main
```
---

## Étape 12 : Lancer le build
1. **Jenkins →** Ton pipeline → **Build Now**
2. **Clique sur le numéro du build**
3. **Observe la Console Output**

---

## Étape 13 : Voir les résultats dans SonarQube
**Pendant que le build tourne :**

1. **Retourne sur SonarQube** : http://localhost:9000
2. **Clique sur ton projet** : `Java Products Lab`
3. **Tu devrais voir** :
   *    **📊 Bugs** : Nombre de bugs détectés
   *    **🔒 Vulnerabilities** : Failles de sécurité
   *    **💩 Code Smells** : Mauvaises pratiques
   *    **📏 Coverage** : Couverture de tests
   *    **🔁 Duplications** : Code dupliqué
   
**🎯 Quality Gate (par défaut) :**

* Le build PASSE si :
    *    Aucun nouveau bug
    *    Aucune nouvelle vulnérabilité
    *    Couverture sur nouveau code > 80%


----
📊 **CE QUE TU DOIS VOIR**

**Console Output :**

```{}text
[Pipeline] stage (SonarQube Analysis)
================================================
ETAPE 3 : Analyse de la qualite du code
================================================
[INFO] --- sonar-maven-plugin:x.x.x:sonar (default-cli) @ java-products-lab ---
[INFO] User cache: C:\Users\ericn\.sonar\cache
[INFO] SonarQube version: 10.x.x
[INFO] Analyzing on SonarQube server: http://localhost:9000
...
[INFO] ANALYSIS SUCCESSFUL
Analyse SonarQube terminee

[Pipeline] stage (Quality Gate)
================================================
ETAPE 4 : Verification du Quality Gate
================================================
Checking status of SonarQube task...
SonarQube task status: SUCCESS
Quality Gate status: PASSED
Quality Gate passe avec succes

[Pipeline] stage (Archive)
...
Finished: SUCCESS
```
---

## ✅ VALIDATION DU MODULE 3
**Réponds-moi avec :**

1. **✅ Statut du build** : SUCCESS ou FAILURE ?
2. **📊 Dans SonarQube :**
    *    Combien de **Bugs** détectés ?
    *    Combien de **Code** Smells ?
    *    **Quality Gate :** Passed ou Failed ?
3. **📸 Capture d'écran** (ou description) de la page SonarQube du projet
4. **❓ Questions** sur SonarQube ou le Jenkinsfile ?

**Prochaine étape (Module 4) :**

*    Installation de **Docker Desktop** sur Windows
*    Création d'un **Dockerfile** pour ton app Spring Boot
*    **Build de l'image Docker** via Jenkins
*    **Push sur Docker Hub**
**Dis-moi "OK MODULE 3" quand c'est validé ! 🎯**

---
     
### 🚀 MODULE 3.5 : AUTOMATISATION & TRIGGERS (BONUS)

**Objectif :**
À chaque git push sur GitHub → Jenkins lance automatiquement le build (sans cliquer sur "Build Now").
---

## Étape 1 : Configuration du Webhook dans GitHub
1. **Va sur GitHub** → Ton repo java-products-lab
2. **Settings → Webhooks** (menu de gauche)
3. **Clique sur "Add webhook"**
4. **Remplis le formulaire :**


| **champ**        |                   **Valeur**                    |  
|:-----------------|:-----------------------------------------------:|
| **Payload URL**  | `http://VOTRE_IP_PUBLIQUE:8080/github-webhook/` | 
| **Content type** |               `application/json `               |     
| **Secret**       |          (laisse vide pour l'instant)           |        
| **Which events** |             `Just the push event `              |        
| **Active**       |                     ✅ Coché                     |        

**⚠️ PROBLÈME : Tu es en LOCAL (localhost)**


GitHub ne peut pas envoyer de webhook à http://localhost:8080 car c'est ta machine personnelle.

---

## Solutions pour tester en local :
**Option A : Ngrok (Tunnel temporaire - RECOMMANDÉ pour apprendre)**
**Ngrok** crée un tunnel public vers ton Jenkins local.

1. **Télécharge Ngrok :** https://ngrok.com/download
2. Dézipper dans c:/ngrok
3. **Extrait et lance :**
```{}Bash
ngrok http 8080
```
4. **Copie l'URL publique** (ex: https://a1b2c3d4.ngrok.io)
5. **Dans GitHub Webhook**, utilise
```{}Bash
https://a1b2c3d4.ngrok.io/github-webhook/
```
✅ Avantages :

*  Gratuit pour tester
* Fonctionne immédiatement
* Parfait pour apprendre
❌ Inconvénients :

*  L'URL change à chaque redémarrage
*  Session limitée (2h en version gratuite)

---

**Option B : Configuration réseau (Avancé)**
Si tu as une IP publique fixe et que tu maîtrises la redirection de ports sur ton routeur :

1. Configure le port forwarding sur ton routeur :
*   Port externe : 8080
*   Port interne : 8080
*   IP locale : Ton PC (ex: 192.168.1.x)
2. Trouve ton IP publique : https://www.whatismyip.com
3. Utilise dans GitHub :

```{}text
http://VOTRE_IP_PUBLIQUE:8080/github-webhook/
```

⚠️ **Risque de sécurité :** Ne pas laisser Jenkins exposé publiquement sans sécurité renforcée.

---

## Option C : Poll SCM (Alternative sans webhook)
**Si tu ne peux pas utiliser de webhook**, Jenkins peut **vérifier GitHub régulièrement :**

**Dans ton pipeline Jenkins :**

1. **Configure** → Section **"Build Triggers"**
2. **Coche :** Poll SCM
3. **Schedule** (syntaxe Cron) :

```{}text
H/5 * * * *
```
→ Vérifier GitHub **toutes les 5 minutes**

**Syntaxe Cron expliquée :**

```{}text
H/5 * * * *
│  │ │ │ │
│  │ │ │ └─ Jour de la semaine (0-7, 0 et 7 = Dimanche)
│  │ │ └─── Mois (1-12)
│  │ └───── Jour du mois (1-31)
│  └─────── Heure (0-23)
└────────── Minute (0-59, H = Hash pour répartir la charge)
```

✅ Avantages :

*   Pas besoin de webhook
*   Fonctionne en local
*   Simple à configurer
❌ Inconvénients :

*   Délai de 5 minutes max
*   Consomme des ressources (vérifications fréquentes)

---

## Étape 2 : Activation du déclencheur dans le Jenkinsfile

**Ajoute ce bloc AVANT** `stages` :
```{}groovy
pipeline {
    agent any
    
    triggers {
        githubPush()  // Webhook GitHub
        // OU
        pollSCM('H/5 * * * *')  // Vérification toutes les 5 min
    }
    
    tools {
        jdk 'JDK17'
        maven 'MAVEN_3.6.3'
    }
    
    // ... reste du code
}
```

📖 **Explication :**


| **Déclencheur**           |                                   **Comportement**                                    |  
|:--------------------------|:-------------------------------------------------------------------------------------:|
| `githubPush()`            |                     Lance le build quand GitHub envoie un webhook                     | 
| `pollSCM('H/5 * * * *')`  |          Vérifie GitHub toutes les 5 minutes et lance le build si changement          | 



**📌 POINT 2 : NOTIFICATIONS (Email/Slack)**
**Option A : Notifications par Email**
## Étape 1 : Configuration SMTP dans Jenkins

1. Jenkins → Manage Jenkins → Configure System
2. Descends jusqu'à "Extended E-mail Notification"
3. Remplis (exemple Gmail) :

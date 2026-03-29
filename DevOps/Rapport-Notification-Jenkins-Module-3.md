# 📝 Rapport de Résolution : Notifications Jenkins (Module 3)
1. **Échec de l'Option A :** Notifications par Email
*   **Problème rencontré :** Erreur `SSLHandshakeException / PKIX path building failed`.
*    **Cause technique :** Le JDK Java installé sur Windows ne possédait pas les certificats racines nécessaires pour valider la connexion sécurisée vers Gmail. 
      C’est un conflit de "chaîne de confiance" entre ton système local et les serveurs Google.
*    **Difficulté :** Importer manuellement des certificats dans le magasin de clés Java (cacerts) est une procédure lourde et risquée pour un environnement de Lab.

---
2. **Transition vers l'Option B : Notifications Slack**
*    **Choix stratégique :** Abandon de l'email pour Slack, une méthode plus moderne, basée sur une API Web (HTTP), évitant ainsi les complexités du protocole SMTP et `des certificats SSL Java`.
3. **Difficultés rencontrées sur Slack**
*    **Erreur 1 :** invalid_auth. Cause : Mauvaise configuration du Token ou de l'URL Webhook.
*    **Erreur 2 :** Connection Refused. Cause : Tentative d'envoi alors que les services (ou Ngrok) n'étaient pas synchronisés.
*    **Erreur 3 :** GitHub Push Protection. Cause : Sécurité de GitHub qui bloque le git push car l'URL secrète était écrite en clair dans le code.
---
4. **La Solution Finale (Ma Victoire)**
*    **Méthode :** Utilisation du plugin Slack Notification avec un Credential propre.
*    **Actions clés :**
   1. **Création du Secret :** Dans Jenkins, création d'un Secret Text contenant uniquement le token d'intégration fourni par Slack.
   2. **Configuration Système :** Liaison du Workspace (nyerdi-devops) avec le Credential ID créé, sans utiliser d'URL d'Override.
   3. **Validation :** Succès du bouton "Test Connection" qui a déclenché le message "You're all set".
   4. **Pipeline-as-Code :** Utilisation de la fonction slackSend dans le Jenkinsfile, rendant le pipeline propre, sécurisé et automatique.
---
```bash
fichier final jenkinsFile "pipeline {
agent any
// 1. Les Triggers (L'allumage)
triggers {
githubPush()  // Webhook GitHub
pollSCM('H/5 * * * *')  // Vérification toutes les 5 min
}
tools {
jdk 'JDK17'
maven 'MAVEN_3.6.3'
}

    environment {
        APP_NAME = 'java-products-lab'
        JAVA_VERSION = '17'
        BUILD_VERSION = "${env.BUILD_NUMBER}"
        SONAR_PROJECT_KEY = 'Java-Products-Lab'
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
                bat 'mvn clean package'
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
                         -Dsonar.java.source=${JAVA_VERSION} ^
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
            slackSend(
                    color: 'good',
                    message: "✅ Build SUCCESS : ${APP_NAME} #${env.BUILD_NUMBER}\n<${env.BUILD_URL}|Voir les détails>"
            )
        }

        failure {
            echo '================================================'
            echo 'PIPELINE ECHOUE !'
            echo "Build #${env.BUILD_NUMBER} a echoue"
            echo 'Cause possible : Quality Gate non respecte'
            echo'='
            echo '================================================'
            slackSend(
                    color: 'danger',
                    message: "❌ Build FAILED : ${APP_NAME} #${env.BUILD_NUMBER}\n<${env.BUILD_URL}console|Voir les logs>"
            )
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

**Conclusion :**
Le  guide sur Slack/Jenkins est une bonne base, mais il manque des précisions critiques :
il faut impérativement cliquer sur "Save Settings" tout en bas de la page Slack pour activer l'URL.
De plus, renseigner l'URL complète au lieu du seul Token dans les Credentials crée des erreurs d'authentification (invalid_auth).
Enfin, l'écriture de l'URL en clair dans le Jenkinsfile est désormais bloquée par la Push Protection de GitHub pour raisons de sécurité.
Il serait plus juste de recommander l'usage exclusif du Secret Text via son ID. 

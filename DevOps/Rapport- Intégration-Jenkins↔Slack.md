# 📘 Guide Corrigé : Intégration Jenkins ↔ Slack (Méthode Fiable)
> **Note de retour d'expérience :** Le guide initial manquait de précision sur la différence entre **l'URL complète** et le** Token secret**. Cela a causé des erreurs invalid_auth. Voici la procédure exacte pour l'interface actuelle.

---
## Étape 1 : Côté Slack (Le Récepteur)
1. **Workspace & Canal :** Crée ton espace (ex: `nyerdi-devops`) et le canal #jenkins-builds.
2. **L'App "Incoming Webhooks" :** Ne crée pas une application complexe "from scratch". Utilise l'application **Legacy Incoming Webhooks** (plus simple pour Jenkins).
      1. Lien direct : `https://[ton-workspace]://`
3. Le Secret (Crucial) : Une fois le Webhook généré, ne copie pas toute l'URL. Identifie le Token d'intégration (la dernière partie après le dernier slash).
      1. Exemple : Si l'URL est `.../services/T01/B02/ABC125`, le token est **ABC125.**
4. **Activation : Obligation** de cliquer sur le bouton vert **"Save Settings"** en bas de la page Slack, sinon le lien reste inactif.

---
## Étape 2 : Côté Jenkins (L'Émetteur)
1. Gestion des Secrets (Credentials) :
1. **Ajoute un Secret `Text.**`
2. **Secret :** Colle uniquement le Token (ex: `ABC125`).
3. **ID :** Donne un nom clair comme `slack-token`.
2.  **Configuration Système ** (`Manage Jenkins` > `System`) :
1. **Workspace **: Tape le sous-domaine (ex: `nyerdi-devops`).
2. **Credential ** : Sélectionne ton `slack-token`.
3. **Default Channel ** : `#jenkins-builds`.
3. **Test de Connexion **: Clique sur "Test Connection". Si Slack affiche "You're all set", c'est gagné.

---
##Étape 3 : Le Jenkinsfile (Sécurité & Automatisation)
*  **Alerte Sécurité** : Ne jamais écrire l'URL Webhook en clair dans le code. GitHub possède une **"Push Protection"** qui bloquera ton commit pour protéger tes données.
*  **Utilisation** : Utilise la fonction slackSend qui s'appuie sur la configuration globale faite à l'étape 2.
---


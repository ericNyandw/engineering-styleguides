# 📂 Guide Complet : Repomix (ex-Repopack)

**Repomix** est un outil en ligne de commande (CLI) qui permet de transformer tout ou partie d'un projet de code en un fichier unique structuré. Ce fichier est optimisé pour être lu par des IA telles que **ChatGPT, Claude ou Gemini**.

---

## 🚀 1. Commandes de base (Démarrage rapide)

L'outil s'utilise sans installation préalable via `npx`.
> ⚠️ **Important :** Lancez toujours ces commandes à la **racine** de votre projet (là où se trouve le dossier `src` ou `pom.xml`).

*   **Générer tout le projet :** `npx repomix`
*   **Compter les tokens (sans générer de fichier) :** `npx repomix --token-count-tree`
*   **Initialiser un fichier de configuration :** `npx repomix --init`

---

## 🛠 2. Exemples de cas d'usage précis

| Objectif | Commande |
| :--- | :--- |
| **Cibler des dossiers spécifiques** | `npx repomix --include "src/main/java/**/entities/**,src/main/java/**/enums/**"` |
| **Nommer le fichier de sortie** | `npx repomix -o myrepomix-out-entities.xml` |
| **Réduire la taille (Compression)** | `npx repomix --compress` (supprime espaces et commentaires) |
| **Copier au presse-papier** | `npx repomix --copy` |

---

## ⚙️ 3. Automatisation avec `repomix.config.json`

L'utilisation d'un fichier de configuration est cruciale pour les projets complexes. Cela évite de retaper manuellement les longs chemins Java et garantit que toute l'équipe utilise les mêmes réglages.

Créez un fichier **`repomix.config.json`** à la racine de votre projet :

```json
{
  "output": {
    "filePath": "myrepomix-out-entities.xml",
    "style": "xml",
    "removeComments": true,
    "removeEmptyLines": true
  },
  "include": [
    "src/main/java/be/nyerdi/apiDreamDiary/entities/**",
    "src/main/java/be/nyerdi/apiDreamDiary/enums/**"
  ],
  "ignore": {
    "customPatterns": [
      "target/**",
      "*.log",
      ".idea/**"
    ]
  }
}
```
---
## 🤖 4. Prompt Engineering (Format d'instruction)
Le format XML est recommandé car les balises` <file path="..."> ` permettent à l'IA de ne pas confondre les classes Java entre elles.
Conseil pour les gros volumes (> 500k tokens) :
Collez le contenu du fichier dans l'IA avec ce message d'introduction :
>Je vais te fournir un fichier XML contenant la structure et le code de mes entités et enums Java.
>Analyse ces données pour comprendre mon modèle de domaine, mais ne réponds rien pour l'instant. Attends mes instructions spécifiques sur les fonctionnalités à implémenter ou les bugs à corriger."

## 💡 Astuces pour les développeurs
*   **Sécurité :** Repomix possède une détection de secrets intégrée (Secret Detection). Il exclura automatiquement les fichiers sensibles comme .env s'ils sont détectés.
*   **Format XML vs Markdown :** Le format XML est recommandé pour les modèles comme Claude 3.5 Sonnet, car les balises <file path="..."> permettent à l'IA de ne pas confondre les différentes classes.
*   **Exclusions :** Par défaut, Repomix respecte votre fichier .gitignore.

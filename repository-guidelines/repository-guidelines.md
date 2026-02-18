# 📂 Gouvernance & Nomenclature des Dépôts

Ce guide définit les standards d'organisation de mon écosystème GitHub. L'objectif est de garantir une **clarté immédiate** pour les collaborateurs et de refléter une **rigueur d'ingénierie**.

---

## 🏗️ 1. Nomenclature Standard
Tous les dépôts doivent suivre la syntaxe **kebab-case** :
- **Minuscules uniquement** (pas de CamelCase ni de majuscules).
- **Tirets `-`** comme séparateurs (pas d'espaces ni d'underscores).
- **Format** : `[contexte]-[nom]-[type]`

## 🗂️ 2. Classification par Pôles d'Expertise

Chaque projet doit être classé dans l'un des pôles suivants pour assurer la cohérence du portfolio :

### A. La Suite "Blueprint" (Produits Industriels)
Projets hautement structurés destinés à être réutilisables ou mis en production.
- *Exemple :* `blueprint-admin-core`, `blueprint-time-track`.

### B. Les Laboratoires (Apprentissage & Veille)
Dépôts dédiés à l'expérimentation technique et à la maîtrise de nouveaux concepts.
- *Suffixe :* `-lab`
- *Exemple :* `angular-rxjs-lab`, `java-fundamentals-lab`.

### C. L'Infrastructure (DevOps & Environnements)
Tout ce qui concerne la virtualisation, l'automatisation et la sécurité système.
- *Suffixe :* `-box`
- *Exemple :* `vagrant-docker-dev-box`, `kali-wifi-audit-box`.

### D. Les Showcase (Évolution & Rétrospective)
Monorepos regroupant plusieurs versions d'une même technologie pour montrer la progression.
- *Suffixe :* `-showcase`
- *Exemple :* `angular-evolution-showcase`.

## 📝 3. Standards de Documentation (README)
Un dépôt professionnel doit répondre aux questions suivantes dès la première lecture :
1. **Statut** : Le projet est-il en Recherche (WIP), Stable, ou Archivé ?
2. **Objectif** : Quelle problématique métier ou technique résout-il ?
3. **Stack** : Quelles sont les versions précises des outils (ex : Angular 20, Java 21) ?

## 🔢 5. Stratégie de Versioning

Pour maintenir des noms de dépôts propres et durables, la version logicielle ne doit jamais figurer dans le titre du repository (sauf cas exceptionnel de Monorepo d'évolution).

### ❌ À éviter (Mauvaises pratiques)
- `mon-projet-v1`, `mon-projet-v2`
- `api-dream-diary-final`, `api-dream-diary-new`

### ✅ À appliquer (Bonnes pratiques)
On utilise les **Git Tags** et les **Releases** GitHub pour marquer les étapes importantes du projet.

| Type de version     | Méthode recommandée                  | Exemple de commande                      |
|:--------------------|:-------------------------------------|:-----------------------------------------|
| **Version majeure** | Créer un Tag Git (SemVer)            | `git tag -a v1.0.0 -m "Release stable"`  |
| **Évolution Tech**  | Branche dédiée (si migration longue) | `git checkout -b feature/migration-ng20` |
| **Rétrospective**   | Monorepo de type `-showcase`         | `/v11-legacy`, `/v20-modern`             |

### 💡 Cas particulier : Le suffixe de technologie
Il est autorisé d'ajouter la technologie principale dans le nom si tu possèdes plusieurs implémentations du même concept :
- `blueprint-admin-angular`
- `blueprint-admin-react` (si tu décides d'en faire une version React un jour)

### 🚀 Cycle de vie d'un projet
1. **Init** : Nom neutre et définitif dès le départ.
2. **Dev** : Travail sur branches `feature/` ou `fix/`.
3. **Milestone** : Création d'une **Release GitHub** pour figer une version dont on est fier.

---
### 🔄 Comparatif : Mauvaises vs Bonnes Pratiques

| Mauvaise Pratique (Scolaire) | Bonne Pratique (Industrielle) | Pourquoi ?                                                                 |
|:-----------------------------|:------------------------------|:---------------------------------------------------------------------------|
| `apprendreJava_001`          | `java-fundamentals-lab`       | On remplace le numéro de série par l'objectif technique (`lab`).           |
| `admin-Dashboard-v2`         | `blueprint-admin-static`      | On remplace la version par la fonction du projet dans l'écosystème.        |
| `apiDreamDiary-final`        | `dream-journal-api`           | Le mot "final" est banni. On utilise les **Tags** pour marquer les étapes. |
| `learn-rxjs-angular11`       | `angular-rxjs-lab`            | La version technique (v11) va dans le `README`, pas dans le nom.           |

### 🛠️ Comment marquer une version proprement ?
Au lieu de changer le nom du dossier, utilise la puissance de **Git** depuis ton terminal :

1. **Pour figer une version 1.0** :
   `git tag -a v1.0.0 -m "Première version stable"`
2. **Pour envoyer la version sur GitHub** :
   `git push origin v1.0.0`

> *Résultat : GitHub créera automatiquement une section **"Releases"** sur le côté de ton projet. C'est beaucoup plus propre et professionnel.*

*Document de référence pour la gestion du cycle de vie des projets.*

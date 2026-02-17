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
3. **Stack** : Quelles sont les versions précises des outils (ex: Angular 20, Java 21) ?

## 🤖 4. Compatibilité IA (Claude Code)
Pour faciliter le travail avec des agents comme **Claude Code**, chaque projet doit maintenir une structure compatible avec **Repomix** pour permettre une analyse de contexte rapide et précise.

---
*Document de référence pour la gestion du cycle de vie des projets.*

# 📂 Guide engineering complet : Codegen & Contract-First (Angular)

Ce guide définit les standards pour la consommation d'APIs tierces via la génération de code automatisée. L'objectif est de garantir une synchronisation parfaite entre le contrat **OpenAPI** et notre implémentation **Angular 20+**.

---

## 🏗️ 1. Philosophie & Approches de Génération

### 1.1. Pourquoi automatiser le client API ?
- **Type Safety** : En Java, vous avez des classes ; en TypeScript, nous utilisons des interfaces. Automatiser la génération garantit que si le backend change un `Long` en `String`, votre frontend s'arrêtera de compiler immédiatement.
- **Zéro maintenance manuelle** : Écrire des services `HttpClient` à la main est une tâche répétitive et sujette à l'erreur humaine.

> 💡 **Note d'expert** : Si votre API est au format **OpenAPI 3**, privilégiez **`ng-openapi-gen`**. Il est conçu spécifiquement pour Angular et gère mieux les modèles complexes.

---

## 🛠️ 2. Mise en œuvre Étape par Étape (Le "Pourquoi")

### Étape 1 : Préparer le fichier Swagger
**Action** : Placer le `petstore.json` dans `src/assets/`.  
**Utilité** : C'est votre **Source de Vérité**. Le projet devient "autonome" : n'importe quel développeur possède le contrat et peut régénérer le client.

### Étape 2 : Configuration du CLI
**Action** : Fixer la version via `npx @openapitools/openapi-generator-cli version-manager set 7.2.0`.  
**Utilité** : Éviter l'effet "ça marche chez moi". On s'assure que le code généré est identique sur toutes les machines et en CI/CD.

### Étape 3 : Scripts de génération dans `package.json`
**Action** : Créer des scripts isolés pour chaque API.  
**Localisation** : Le code généré est placé dans **`src/app/api/`**. Chaque API possède son propre sous-dossier (ex : `/api/petstore`, `/api/billing`).  
**Utilité** : Isoler les contrats. Cela évite que les modèles de deux APIs différentes ne s'écrasent.

```json
"scripts":{
  "generate:petstore": "openapi-generator-cli generate -i src/assets/petstore.json -g typescript-angular -o src/app/api/petstore --additional-properties=providedInRoot=true",
  "generate:billing": "openapi-generator-cli generate -i src/assets/billing.json -g typescript-angular -o src/app/api/billing --additional-properties=providedInRoot=true"
}
```
>Note : providedInRoot=true est crucial en Angular moderne pour injecter les services sans imports de modules.

>💡 La bonne pratique : Pourquoi placer le code dans src/app/api/ ?
> Bien que ce soit du code généré, il fait partie intégrante de la logique de consommation de votre application. Le placer sous src/app/ permet à Angular (et à votre IDE) de résoudre les imports facilement tout en gardant une séparation nette entre votre code métier et le SDK de l'API.

### 🤖 3. Architecture Moderne & Configuration
### 3.1. Configuration dans app.config.ts (Multi-API & Aliasing)
**Action** : Utiliser des alias pour injecter plusieurs BASE_PATH sans conflit.

```typescript
import { BASE_PATH as PETSTORE_PATH } from './api/petstore';
import { BASE_PATH as BILLING_PATH } from './api/billing';

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(withFetch()),
    { provide: PETSTORE_PATH, useValue: 'https://petstore.swagger.io' },
    { provide: BILLING_PATH, useValue: 'https://api.billing-system.com' }
  ]
};
```
**Pourquoi cette méthode ?**
- **Indépendance** : Chaque service généré (ex : PetService) ira chercher sa propre URL associée à son alias.
- **Clarté** : Centralisation totale de la configuration réseau.

### 3.2. Couche de Service métier (Service Layer)
**Action** : Encapsuler l'API générée dans un service local.
**Utilité** : L'Isolation. Si vous changez d'outil de génération, vous ne modifiez que votre UserService. Les **Signals** assurent une interface réactive.

**Exemple d'utilisation dans un composant :**

```typescript
export class PetComponent {
private petApi = inject(PetApiService); // Service généré
public pet = signal<Pet | null>(null);

constructor() {
this.petApi.getPetById(1).subscribe(data => this.pet.set(data));
}
}
```

### ⚙️ 4. Organisation Technique & DTOs
### 1. Le "Barrel Export" (Fichier index)
   **Action** : Centraliser les modèles dans src/app/core/models/index.ts.
   **Utilité** : Garder des imports propres.
   ```typescript
   // src/app/core/models/index.ts
   export * from '../../api/petstore/model/models';
   export * from '../../api/billing/model/models';
 ```

 #### 2. Extension pour l'UI (View Models)
**Action** : Créer des interfaces qui étendent les DTOs générés.
**Utilité** : Ajouter des propriétés spécifiques à l'interface (ex : isSelected) sans modifier le code généré.
   ```typescript
   import { Pet } from '@api/models';

export interface PetUI extends Pet {
isSelected: boolean;
}
```

### 3. Stockage & Exclusion
   **Action** : Ajouter le dossier généré au.gitignore.
   **Utilité** : Ne pas polluer Git. Le code généré est un artefact de build, comme le dossier target/ en Java.
   
### 💡 5. Standards de Développement
   - **Zéro any** : En TypeScript, any désactive la vérification. À bannir absolument.
   - **Service Abstraction** : Un composant ne doit pas savoir comment on récupère la donnée, il veut juste la consommer via un Signal.
   ## 🚀 Étude de cas réelle
Consultez l'implémentation complète (Angular 20, Multi-API, Signals) :
   👉 **angular-codegen-lab**


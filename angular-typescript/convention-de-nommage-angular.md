**[Convention de nommage Angular]{.underline}**

Ce résumé combine les conventions de nommage standard d\'Angular avec
l\'explication sur l\'obsolescence de l\'underscore (\_) en TypeScript
moderne, qui est la base d\'Angular.

| Élément                                 |                      Convention de nommage                      |                                                                                                                                      Example et explication |
|:----------------------------------------|:---------------------------------------------------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| **Fichiers**                            |  **kebab-case**, avec un suffixe pour le type. user.service.ts  |                                                                                                **<p>user-profile.component.ts**<br/>**user.service.ts**</p> |
| **Classes**                             |        **PascalCase**, avec un suffixe   conventionnel.         |                                                       **UserProfileComponent, DataService** </br> **Raison :** Lisibilité etdistinction claire des classes. |
| **Sélecteurs**                          |        **kebab-case**, avec un préfixe  d\'application.         |                                                               **<app-user-profile\>** </br>  **Raison :** Évite les conflits de noms avec les balises HTML. |
| **Propriétés/Méthodes (publiques)**     |                          **camelCase**                          |                                                                 **userData**,**fetchData()** </br> **Raison :** Convention standard de JavaScript/TypeSprit |
| **Observables**                         |              **camelCase** se terrminant par un $.              |                                                               **userData$** </br> **Raison :** Convention forte pour identifier un flux de données(stream). |
| **Propriétés/Méthodes (publiques)**     |        **private et camelCase**.  **N'utilisez pas _**.         |                        **private dataService: DataService;** </br>**Raison :** TypeScript gère l\'encapsulation.</br> Le **_** est une convention obsolète. |
| **Injection de dépendances**            | **private et camelCase**dans le constructor **N'utilisez pas_** | **constructor(private dataService:DataService){ ... }** </br> **Raison :** L'encapsulation est gérée par le mot-clé private, pas par le nom de la variable. |


**Exemples détaillés combinés**

**Exemples détaillés combinés**

**1. Fichier de service (data.service.ts)**

code in TypeScript :
 ```
import { Injectable } from \'@angular/core\';
import { Observable, of } from 'rxjs';

@Injectable({providedIn: 'root'})
export class DataService {

*// Propriété privée en camelCase. Le mot-clé `private` gère l'accès.*
private users = ['Alice', 'Bob', 'Charlie'];
*// Méthode publique en camelCase.*
 getUsers(): Observable\<string\[\]\> {
   return of(this.users);
 }
}

```
N.B:Utilisez le code avec précaution.



**2. Fichier de composant (user-profile.component.ts)**


code in TypeScript :
 ```
import { Component, OnInit } from '@angularcore';
import { DataService } from '../data.service';
import { Observable } from 'rxjs';

@Component({
selector: 'app-user-profile', *// Sélecteur en kebab-case*
template: `
           <ul\>
           <li *ngFor="let user of users$ | async">{{ user }}</li\>
           </ul\>
         `})
 ```

 ```
export class UserProfileComponent implements OnInit { *// Classe en
 ```
N.B : PascalCase*
 ```
*// Injection de dépendance via le constructeur.*
*// La propriété `dataService` est privée et en camelCase.*

constructor(private dataService: DataService) { }

*// Observable en camelCase se terminant par.*

users$: Observable<string[]>;
ngOnInit(): void {
  *// Appel à la méthode du service.*
   this.users$ = this.dataService.getUsers();
  }
}
 ```

N.B: Utilisez le code avec précaution.

**Synthèse**

L\'usage des conventions de nommage d\'Angular, en conformité avec les
bonnes pratiques de TypeScript, est essentiel pour la lisibilité et la
maintenabilité d\'un projet. Il est crucial de retenir que le
préfixe \_ pour les attributs privés est une pratique révolue. Le
mot-clé private remplit cette fonction de manière beaucoup plus fiable
et conforme aux standards modernes.

oici des exemples détaillés de la convention de
nommage**kebab-case**telle qu\'elle est appliquée dans un projet
Angular.

Le kebab-case est une convention où tous les mots sont en minuscules et
séparés par des tirets (-). En[Angular]{.underline}, cette convention
est principalement utilisée pour les noms de fichiers et les sélecteurs
de composants et de directives.

**1. Nommage des fichiers et des dossiers**

Chaque fichier et dossier doit être nommé en kebab-case. Cela
s\'applique à tous les types de fichiers (TypeScript, HTML, CSS, etc.)
et à leurs noms de test correspondants.

**Exemples :**

- **user-profile** (dossier)

- **user-profile.component.ts** (fichier TypeScript du composant)

- **user-profile.component.html** (template HTML du composant)

- **user-profile.component.css** (fichier de style du composant)

- **user-profile.service.ts** (service)

- **auth.guard.ts** (garde de route)

- **date-format.pipe.ts** (pipe)

- **highlight.directive.ts** (directive)

- **app.module.ts** (module principal)

- **app.routes.ts** (fichier de routes)

- **user-profile.component.spec.ts** (fichier de test unitaire pour le
  composant)

**2. Sélecteurs de composants**

Le sélecteur d\'un composant doit être en kebab-case. Il est recommandé
d\'ajouter un préfixe, généralement app-, pour éviter les conflits de
noms avec les balises HTML natives.

**Exemples :**

code in TypeScript :

 ```
*// Fichier : user-profile.component.ts*
 import { Component } from '@angularcore';

@Component({
selector: 'app-user-profile',  *// Sélecteur en kebab-case*
templateUrl: './user-profile.component.html',
styleUrls: ['./user-profile.component.css']
})
export class UserProfileComponent {} *// La classe est en PascalCase*
 ```

N.B: Utilisez le code avec précaution.

Utilisation dans le template html :

 ``` 
*<!-- Fichier : app.component.html -->*
<app-user-profile> </app-user-profile>
 ```

N.B: Utilisez le code avec précaution.

**3. Sélecteurs de directives**

Pour les directives, la convention est similaire à celle des composants,
mais il est important de distinguer s\'il s\'agit d\'une directive
d\'attribut ou d\'élément.

**Directive d\'attribut (camelCase pour l\'attribut, kebab-case pour la
classe)**

Les sélecteurs de directives d\'attribut utilisent le camelCase pour le
nom de l\'attribut dans le code, mais sont écrits en kebab-case dans le
template.

code in TypeScript :

 ```
*// Fichier : highlight.directive.ts*
import { Directive, ElementRef, Input, OnInit } from '@angularcore';
@Directive({
selector: '[appHighlight]' *// Sélecteur d'attribut en camelCase*
})
export class HighlightDirective implements OnInit {

constructor(private el: ElementRef) {}

ngOnInit() {
           this.el.nativeElement.style.backgroundColor = \'yellow\';
       }
}
 ```

N.B: Utilisez le code avec précaution.

Utilisation dans le template (l\'attribut est en kebab-case pour
l\'HTML) :

HTML: template

 ```
 <p app-highlight\> Ce texte sera surligné.</p>
 ```

N.B : Utilisez le code avec précaution.

**Directive d\'élément**

Si la directive est utilisée comme un élément (moins fréquent), son
sélecteur sera en kebab-case, comme pour un composant.

code in TypeScript :

```
@Directive({
selector:'app-fancy-directive'
})
export class FancyDirective {}
 ```

N.B :Utilisez le code avec précaution.

**4. Entrées et sorties (Input/Output) avec alias**

Il est recommandé d\'utiliser kebab-case pour les alias dans les
templates afin de respecter les conventions HTML.

code in TypeScript :

 ``` 
*// Fichier : my-child.component.ts*
import { Component, Input } from '@angularcore';

@Component({
selector: 'app-my-child',
template: `<h1>{{ userFirstname }}</h1>`
})
export class MyChildComponent {
* //L'alias est en kebab-case dans le template.*
@Input('user-first-name') userFirstname: string;

}
 ```

Utilisez le code avec précaution.

Utilisation dans le template parent :

 ```
html
*<!-- Fichier : my-parent.component.html -->*
<app-my-child user-first-name="Jean"></app-my-child>
 ```

Utilisez le code avec précaution.

**5. Avantages du kebab-case**

- **Compatibilité HTML :**L\'utilisation de kebab-case pour les
  sélecteurs de composants assure la compatibilité avec le standard
  HTML, car il utilise des noms de balises minuscules avec des tirets.

- **Uniformité :**Cela crée une cohérence visuelle et structurelle
  dans l\'ensemble du projet, en séparant clairement les classes
  (PascalCase) des noms de fichiers et sélecteurs (kebab-case).

- **Génération automatique :**L\'interface de ligne de commande (CLI)
  d\'Angular utilise par défaut le kebab-case lors de la génération de
  fichiers et de composants, encourageant cette convention dès le
  départ. 

# 📂 Guide engineering complet : Codegen & Contract-First (Java)

Ce guide définit les standards pour la consommation d'APIs tierces via la génération de code automatisée. L'objectif est de garantir une synchronisation parfaite entre le contrat **OpenAPI** et notre implémentation **Spring Boot 3**.

---

## 🏗️ 1. Philosophie & Approches de Génération

### 1.1. Approche Industrielle : Plugin Maven (Recommandée)
Le code est généré automatiquement à chaque build du projet. Cela garantit que le client est toujours à jour avec le fichier de spécification.

### 1.2. Approche Ponctuelle : CLI Swagger Codegen
Utilisée pour des tests rapides ou des scripts hors environnement de build :
- **Télécharger le JAR** : [Swagger Codegen CLI](https://repo1.maven.org)
- **Commande** :  
  `java -jar swagger-codegen-cli.jar generate -i swagger.json -l java --library resttemplate -o ./client-output`

---

## 🛠️ 2. Mise en œuvre Étape par Étape

### Étape 1 : Préparer le fichier Swagger
Placez votre fichier de spécification (ex : `petstore.json` ou `api-docs.yaml`) dans le dossier :  
`src/main/resources/`

### Étape 2 : Configurer le `pom.xml`
Ajoutez le plugin `openapi-generator-maven-plugin`. Les options ci-dessous sont cruciales pour l'organisation dans `target/` :

```xml
<configuration>
    <inputSpec>${project.basedir}/src/main/resources/petstore.json</inputSpec>
    <generatorName>java</generatorName>
    <library>webclient</library> <!-- Utiliser webclient ou resttemplate -->
    
    <!-- Organisation des dossiers dans target/ -->
    <apiPackage>com.petstore.client.api</apiPackage>
    <modelPackage>com.petstore.client.model</modelPackage>
    <invokerPackage>com.petstore.client.invoker</invokerPackage>
    
    <configOptions>
        <useJakartaEe>true</useJakartaEe>
        <generateClientAsBean>true</generateClientAsBean>
        <generateApiTests>false</generateApiTests> <!-- Désactiver les tests générés -->
        <generateModelTests>false</generateModelTests>
    </configOptions>
</configuration>
```
### Étape 3 : Générer le code
Exécutez la commande suivante dans le terminal :
```
mvn clean compile
```
Les fichiers seront générés dans : **target/generated-sources/openapi/**.

### Étape 4 : Utiliser le client généré
Exposez les classes de l'API en tant que **Beans Spring** dans une classe @Configuration :
```java
@Bean
public UserApi userApi(ApiClient apiClient) {
return new UserApi(apiClient);
}
```


### 📝 3. Modernisation & Annotations (OpenAPI 3)
Si vous utilisez [Springdoc OpenAPI](https://springdoc.org/), voici les équivalences pour moderniser votre code :

| **Swagger 2 (Ancien)** | **OpenAPI 3 (Nouveau)** |                       **Description** |
|:-----------------------|:-----------------------:|--------------------------------------:|
| @ApiOperation          |     **@Operation**      |    Résumé et description de la route. |
| @ApiParam              |     **@Parameter**      | Détails des paramètres (Path, Query). |
| @ApiModel              |       **@Schema**       |                   Métadonnées du DTO. |


### Exemple de Record DTO documenté
L'utilisation de **Records** est recommandée pour les DTOs immutables :
```java
@Schema(description = "DTO Utilisateur")
public record UserDTO(
@Schema(example = "123") Long id,
@NotBlank @Schema(requiredMode = Schema.RequiredMode.REQUIRED) String name
) {}
```


### ⚙️ 4. Organisation Technique
1. **Stockage des fichiers générés**
   Les fichiers ne doit jamais être modifiés manuellement. Ils résident dans target/. Pour qu'IntelliJ les reconnaisse :
   Allez dans **Project > Appearance > Excluded Files** pour afficher le dossier **target**.
   Clic droit sur **target/generated-sources/openapi/src/main/java** → **Mark Directory as > Generated Sources Root**.
2. **L'API Client (Usage court)**
   **L'Api Client** est le moteur technique. Il s'utilise comme suit :
   ```java
   ApiClient client = new ApiClient();
   client.setBasePath("https://petstore.swagger.io");
   UserApi api = new UserApi(client);
   api.getUserByName("JohnDoe").block(); // Appel via WebClient
   
   ```
💡 5. **Standards de Développement**
**Validation **: Ajoutez toujours la dépendance **Spring Boot-starter-validation** pour activer les contraintes **@Valid** générées.
**Mapping Domain** : Utilisez l'opérateur .map() dans vos services pour transformer les DTOs du dossier target en vos propres objets métier.
**Exclusion Git** : Le dossier **target/** doit être exclu via le**.gitignore**.

### 🚀Étude de cas réelle
Pour voir une implémentation complète et fonctionnelle de ces concepts (Service, Controller, Configuration), consultez le projet lab associé :
👉 [java-codegen-lab](https://github.com/ericNyandw/java-codegen-lab)



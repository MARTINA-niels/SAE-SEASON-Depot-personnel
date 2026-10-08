# Configuration Maven (deux exécutions)

## 1. Exécutions demandées

| Exécution | Commande | Classe | Sortie |
|---|---|---|---|
| Une partie | `mvn compile exec:java@partie` | `fr.seasons.app.MainPartie` | Déroulé visible et compréhensible |
| 500 parties | `mvn compile exec:java@simulation` | `fr.seasons.app.MainSimulation` | Uniquement le résumé global |

Les deux sont aussi configurables par arguments : `-Dexec.args="--joueurs 3 --graine 42"` ; la simulation accepte `--parties 500`.

## 2. Extrait de `pom.xml`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>fr.seasons</groupId>
  <artifactId>seasons</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <junit.version>5.10.2</junit.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.17.1</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.2.5</version>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>3.2.0</version>
        <executions>
          <execution>
            <id>partie</id>
            <goals><goal>java</goal></goals>
            <configuration>
              <mainClass>fr.seasons.app.MainPartie</mainClass>
            </configuration>
          </execution>
          <execution>
            <id>simulation</id>
            <goals><goal>java</goal></goals>
            <configuration>
              <mainClass>fr.seasons.app.MainSimulation</mainClass>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

Notes :

- Aucune phase n'est liée aux exécutions `partie` et `simulation` : elles se lancent à la demande avec `exec:java@id`.
- Jackson sert à lire `cartes.json`, `jeux-preconstruits.json` et `des.json`. Si l'équipe préfère n'avoir aucune dépendance, ces données peuvent être déclarées en Java (classes de fabrique) : la structure du modèle ne change pas.
- La version de Java (21 ici) est à aligner sur celle imposée par le cours.

## 3. Intégration continue (optionnel)

Un workflow GitHub Actions minimal (`.github/workflows/build.yml`) exécute `mvn -B clean verify` à chaque push, pour garantir que chaque livraison du vendredi compile et passe les tests.

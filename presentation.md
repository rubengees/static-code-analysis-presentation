class: center, middle

# Statische Code-Analyse

***von Alexej Esau und Ruben Gees***

---

# Inhalt

- Was ist statische Code-Analyse?

- Einheitlicher Code-Stil mit `Checkstyle`

- Automatisiert Fehler finden mit `FindBugs`

- Code Coverage prüfen mit `JaCoCo`

- Alles zusammenbringen mit `SonarQube`

- Zusammenfassung

- Ausblick

- Quellen

---

# Statische Code-Analyse

.left-column[
## Was?
]

.right-column[
*"Statische Code-Analyse oder kurz statische Analyse ist ein statisches
Software-Testverfahren, das zur Übersetzungszeit durchgeführt wird. Der
Quelltext wird hierbei einer Reihe formaler Prüfungen unterzogen, bei denen
bestimmte Sorten von Fehlern entdeckt werden können, noch bevor die
entsprechende Software (z. B. im Modultest) ausgeführt wird."*

*Quelle: https://de.wikipedia.org/wiki/Statische_Code-Analyse*
]

---

# Statische Code-Analyse

.left-column[
## Was?
]

.right-column[
## Arten

- Code-Stil-Analyse

- Code Smells

- Fehlersuche (potenzieller Fehler)

- Metriken
    - Testabdeckung
    - Komplexizität
    - Wartbarkeit

- Architektur-Analyse

- ...
]

---

# Statische Code-Analyse

.left-column[
## Was?
## Wofür?
]

.right-column[
- Unterstützung bei der Arbeit
    - Automatisierung musterbasierter Routine-Checks
    - Aufdeckung potenzieller Fehlerquellen
    - Aufzeigen von Potenzialen

- Visualisierung der Code-Qualität
]

---

# Statische Code-Analyse

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column.center[
## Mit nachfolgenden Tools ...
]

---

class: center, middle

.title-image[![](img/checkstyle-logo.png)]

---

# Checkstyle

.left-column[
## Was?
]

.right-column[
- Automatisiertes Validieren des (Java-)Code-Stils gegen definierte Regeln

- Aktuelle Version (Juni 2017): `7.8`

- Lizenz: `GNU LGPL v2.1`

- Aktive Weiterentwicklung auf https://github.com/checkstyle/checkstyle

- Monatliches Release (sofern Änderungen vorhanden)

- 100% Coverage!
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
]

.right-column[
- Projektweit einheitlicher Code-Stil

- Vereinfachung der Code-Reviews
]

???

- Jeder Entwickler hat seinen eigenen Stil

- Code-Reviews können sich verstärkt auf inhaltliche Korrektheit konzentrieren
  und müssen sich nicht mehr um simple Stil-Fehler kümmern

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
- Regelwerk via XML konfigurierbar

- Regelwerk

    - Sammlung von Modulen

- Modul

    - Durch Checkstyle ausführbare Einheit

    - Sammlung von Eigenschaften und Modulen

- Eigenschaft

    - Einstellung für das übergeordnete Modul

- Verwendung vordefinierter und/oder eigener Module möglich
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
```xml
<module name="Checker">
  <module name="TreeWalker">
    <module name="MethodLength">
      <property name="tokens" value="METHOD_DEF"/>
    </module>
    <module name="MethodLength">
      <property name="tokens" value="CTOR_DEF"/>
      <property name="max" value="60"/>
    </module>
  </module>
</module>
```

- Alle Module unterstehen dem `Checker`-Modul.

- Wichtige `Eigenschaften`: `baseDir` und `charset`. Es gibt noch viele
[weitere](http://checkstyle.sourceforge.net/config.html#Checker).

- `TreeWalker`-Modul ist das wichtigste

    - Aufruf von Modulen wenn Token gefuden wird
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
- Gewichtung von Modulen

    ```xml
    <module name="Translation">
      <property name="severity" value="warning"/>
    </module>
    ```

- Meldung bei Regelverstoß

    ```xml
    <module name="MemberName">
      <message key="name.invalidPattern"
               value="Member ''{0}'' is wrong!" />
    </module>
    ```

- Eigene Module mit
[RegEx](https://de.wikipedia.org/wiki/Regul%C3%A4rer_Ausdruck)

    ```xml
    <!-- Findet Floats, die nicht  -->
    <!-- dem Format 3f entsprechen. -->
    <module name="RegexpSinglelineJava">
      <property name="format" value="[^\w][\d]+\.f"/>
    </module>
    ```
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
- Einzelne Probleme ignorieren

    ```xml
    <module name="SuppressionFilter">
      <property name="file" value="cs_suppressions.xml" />
    </module>
    ```

- Neue Datei: `cs_suppressions.xml`.

    ```xml
    <suppressions>
      <suppress files="[\w]*Test.java" checks="SomeCheck"/>
    </suppressions>
    ```

- Ignorieren direkt im Code

    ```java
    @SuppressWarnings("checkstyle:nestedifdepth")
    public void someMethod() {
        // Many nested structures.
    }
    ```
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
- Vordefinierte Regelwerke

    - Google: http://checkstyle.sourceforge.net/google_style.html

    - Sun: http://checkstyle.sourceforge.net/sun_style.html
]

---

# Checkstyle

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## Möglichkeit 1: CLI

```bash
java -jar checkstyle.jar -c checkstyle.xml MyClass.java
```

- Zahlreiche Konfigurationsmöglichkeiten:

    - Ausgabeformat mit `-f` (`plain`/`xml`)

    - Ausgabedatei mit `-o`

    - Dateien zum ignorieren mit `-e` oder `-x` (RegEx)

.center[![](img/checkstyle-terminal.png)]
]

---

class: center, middle

# Demo

---

class: center, middle

.title-image[![](img/findbugs-logo.png)]

---

class: center, middle

# Demo

---

class: center, middle

.title-image[![](img/jacoco-logo.png)]

---

class: center, middle

# Demo

---

class: center, middle

.title-image[![](img/sonarqube-logo.png)]

---

layout: true

# SonarQube

---

.left-column[
## Was?
]

.right-column[
## Allgemeines

- Plattform zur Auswertung und Visualisierung von Daten gewonnen durch
    statische Code-Analyse

- Version (Juni 2017): `6.4` bzw. `5.6.6 LTS`

- Lizenz: `GNU LGPL v3.0`

- Entwicklungs-Status: Aktiv (https://github.com/sonarsource/sonarqube)

- Entwicklung durch SonarSource S.A.
    - Seit 2008
]

???

- SonarSource S.A. ist ein schweizer Unternehmen

---

.left-column[
## Was?
]

.right-column[
## Etwas konkreter

- Webapp
    - Visualisierung der Auswertung
    - Management (Benutzer, Projekte, Regeln, Quality Gates, Plugins, Issues, ...)
    - Reviews / Diskussionen

- Datenbank
    - Persistierung

- Scanner
    - Durchführung der Auswertung
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-projects-overview.png)
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-project-details-overview.png)
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-project-details-issues.png)
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-project-details-measures.png)
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-project-details-measures-maintainability.png)
]

---

.left-column[
## Was?
]

.right-column[
## Webapp

![](img/sonarqube-project-details-code.png)
]

---

.left-column[
## Was?
]

.right-column[
## nice2know

- 20+ Code Analyzers
    - `Java`, `C/C++`, `Objective-C`, `Swift`, `C#`, `VB.NET`, `VB6`,
        `PHP`, `XML`, `HTML`, `CSS/Less/Sass`, `JavaScript`, `COBOL`, `ABAP`,
        `SQL`, ...

- 3.000+ Regeln

- Von 80.000+ Organisationen verwendet
]

---

.left-column[
## Was?
]

.right-column[
## Geschmacksrichtungen

- self-hosted
    - https://sonarqube.org/downloads

- Cloud
    - https://sonarqube.com bzw. https://sonarcloud.io
]

---

.left-column[
## Was?
## Wofür?
]

.right-column[
## Webapp

- Zentrale Anlaufstelle
    - Visualisierung von Analysen
    - Sammeln von Analyse-Daten aus statischer Code-Analyse
    - Verwaltung von Regeln und Quality Gates
        - Projektspezifisch (ein Projekt)
        - Projektübergreifend (mehrere Projekte / unternehmensweit?)
    - Reviews / Diskussionen
    - Unabhänigkeit von IDEs / Editoren und Entwicklern
]

---

.left-column[
## Was?
## Wofür?
]

.right-column[
## Scanner

- Analyse des Codes

- Standalone Scanner / Build Tool Scanner
    - Transfer der Analyse zu SonarQube

- IDE-Plugins
    - IDE-interne Visualisierung der Analyse
]

---

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
## Webapp

- Installation notwendiger Scanner-Plugins (Regelwerke)

- Profile erstellen / konfigurieren (Regeln zuordnen)

- Profile den Projekten zuordnen
]

---

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
## Scanner

- Verbindung zum Server konfigurieren

- Scanner starten
]

---

.left-column[
## Was?
## Wofür?
## Wie?
]

.right-column[
## Ablauf

![](img/sonarqube-analyzing-process.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarQube Scanner

![](img/sonarqube-scanners.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## CLI Scanner

```properties
# conf/sonar-scanner.properties

sonar.host.url=https://puck257.startdedicated.de/sonarqube
# Set key or user name to login
sonar.login=MyGeneratedAndVerySecretKey
# Only needed when using user name
#sonar.password=
sonar.projectKey=static-code-analysis-presentation-demo:master
sonar.projectName=static-code-analysis-presentation-demo
sonar.projectVersion=1.0-SNAPSHOT
sonar.sources=src/
sonar.sourceEncoding=UTF-8
```

```bash
# run the scanner
sonar-scanner
```
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## Gradle

```groovy
// build.gradle

buildscript {
    repositories {
        maven { url "https://plugins.gradle.org/m2/" }
        jcenter()
    }

    dependencies {
        classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:2.4"
    }
}

apply plugin: "org.sonarqube"

// ... ->
```
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## Gradle

```groovy
// build.gradle

// ... ->

sonarqube {
    properties {
        property "sonar.host.url", "https://puck257.startdedicated.de/sonarqube"
        property "sonar.login" "MyGeneratedAndVerySecretKey"
        property "sonar.projectName", "static-code-analysis-presentation-demo"
        property "sonar.projectKey", "static-code-analysis-presentation-demo:master"
    }
}
```

```bash
# run the scanner
gradle clean check sonarqube
```
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-settings-plugins.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-settings-plugins-install-plugin.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-settings-configure-server.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-settings-configure-server-connection.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-settings-bind-project.png)
]

---

.left-column[
## Was?
## Wofür?
## Wie?
## Anwendung
]

.right-column[
## SonarLint (IDEA)

![](img/sonarqube-sonarlint-idea-analyze-file.png)
]

---

layout: true

---

class: center, middle

# Demo

---

# Zusammenfassung

_TODO_

---

# Ausblick

_TODO_

---

# Quellen

- https://de.wikipedia.org/wiki/SonarQube
- https://www.sonarqube.org
- https://www.sonarqube.com
- https://www.sonarsource.com
- http://www.sonarlint.org
class: center, middle

# Statische Code-Analyse

***von Alexej Esau und Ruben Gees***

---

# Inhalt

- Was ist statische Code-Analyse?

- Einheitlichen Style überprüfen: `Checkstyle`

- Automatisch Fehler finden: `FindBugs`

- (Unit)-Testabdeckung: `JaCoCo`

- Alles zusammen: `Sonarqube`

- Zusammenfassung

- Ausblick

- Quellen

---

# Was ist statische Code-Analyse?

> *Statische Code-Analyse oder kurz statische Analyse ist ein statisches
> Software-Testverfahren, das zur Übersetzungszeit durchgeführt wird. Der
> Quelltext wird hierbei einer Reihe formaler Prüfungen unterzogen, bei denen
> bestimmte Sorten von Fehlern entdeckt werden können, noch bevor die
> entsprechende Software (z. B. im Modultest) ausgeführt wird.*

*Quelle: https://de.wikipedia.org/wiki/Statische_Code-Analyse*

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

- Aktuelle Version (Mai 2017): `7.7`

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

- Zahlreiche Konfigurationsmöglichkeite:

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

# Sonarqube

_TODO_

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

_TODO_

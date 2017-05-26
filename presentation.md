<!-- class: center, middle -->
# Statische Code Analye

##### Von Alexej Esau und Ruben Gees

---

# Struktur

- Was ist statische Code Analyse?

- Einheitlichen Style überprüfen: `Checkstyle`

- Automatisch Fehler finden: `FindBugs`

- (Unit)-Testabdeckung: `JaCoCo`

- Demo

- Alles zusammen: `Sonarqube`

- Demo

- Zusammenfassung

- Ausblick

- Quellen

---

# Was ist statische Code Analyse?

"Statische Code-Analyse oder kurz statische Analyse ist ein statisches Software-Testverfahren, das zur Übersetzungszeit durchgeführt wird. Der Quelltext wird hierbei einer Reihe formaler Prüfungen unterzogen, bei denen bestimmte Sorten von Fehlern entdeckt werden können, noch bevor die entsprechende Software (z. B. im Modultest) ausgeführt wird."

---

<!-- class: center, middle -->

![](img/checkstyle-logo.png)

---

.left-column[
  ## Was ist das?
]
.right-column[
- Automatisches Überprüfen von (Java-)Code auf einen gegebenen Standart.

- Aktuelle Version: `7.7`

- Unter `GNU LGPL v2.1` lizensiert.

- Aktive Weiterentwicklung auf [GitHub](https://github.com/checkstyle/checkstyle).

- Jeden Monat ein Release wenn es Änderungen gab.
]

---

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
]
.right-column[
- Entwickler haben unterschiedliche Stile, das Projekt soll aber einheitlich sein.

- Entwickler machen Fehler beim Design.

- Kein manuelles Review mehr nötig.
]

---

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
  ## Wie funktioniert das?
]
.right-column[
Checkstyle ist stark konfigurierbar. Es gibt eine zentrale Konfiguration: `checkstyle.xml`.

- Unterteilung in `Module` und `Eigenschaften`.

- `Module` sind die einzelnen Einheiten die `Checkstyle` ausführt. `Eigenschaften`
  beschreiben wie ein `Modul` seine Arbeit erledigen soll.

```xml
<module name="Checker">
    <module name="JavadocPackage"/>
    <module name="TreeWalker">
        <property name="tabWidth" value="4"/>
        <module name="AvoidStarImport"/>
        <module name="ConstantName"/>
        ...
    </module>
</module>
```
]

---

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
  ## Wie funktioniert das?
]
.right-column[
Alle Module unterstehen dem `Checker`-Modul.

- Wichtige `Eigenschaften`: `baseDir` und `charset`. Es gibt noch viele [weitere](http://checkstyle.sourceforge.net/config.html#Checker).

Das `TreeWalker`-Modul erstellt einen Syntaxbaum für jede Datei. Hier finden die meisten Checks statt.

- Jedes Untermodul wird aufgerufen wenn ein definiertes Token gefunden wird

```xml
<module name="MethodLength">
    <property name="tokens" value="METHOD_DEF"/>
</module>
<module name="MethodLength">
    <property name="tokens" value="CTOR_DEF"/>
    <property name="max" value="60"/>
</module>

```
]

---

# FindBugs

_TODO_

---

# JaCoCo

_TODO_

---

<!-- class: center, middle -->
# Demo

##### Checkstyle, FindBugs und Jacoco

---

# Sonarqube

_TODO_

---

<!-- class: center, middle -->
# Demo

##### Sonarqube und CI (Jenkins)

---

# Zusammenfassung

_TODO_

---

# Ausblick

_TODO_

---

# Quellen

_TODO_

<!-- class: center, middle -->
# Statische Code Analye

##### Von Alexej Esau und Ruben Gees

---

# Struktur

- Was ist statische Code Analyse?

- Einheitlichen Style überprüfen: `Checkstyle`

- Automatisch Fehler finden: `FindBugs`

- (Unit)-Testabdeckung: `JaCoCo`

- Alles zusammen: `Sonarqube`

- Zusammenfassung

- Ausblick

- Quellen

---

# Was ist statische Code Analyse?

"Statische Code-Analyse oder kurz statische Analyse ist ein statisches
Software-Testverfahren, das zur Übersetzungszeit durchgeführt wird. Der
Quelltext wird hierbei einer Reihe formaler Prüfungen unterzogen, bei denen
bestimmte Sorten von Fehlern entdeckt werden können, noch bevor die
entsprechende Software (z. B. im Modultest) ausgeführt wird."

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

- Aktive Weiterentwicklung auf
[GitHub](https://github.com/checkstyle/checkstyle).

- Jeden Monat ein Release wenn es Änderungen gab.

- 100% Coverage!
]

---

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
]

.right-column[
- Entwickler haben unterschiedliche Stile, das Projekt soll aber einheitlich
sein.

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
Checkstyle ist stark konfigurierbar. Es gibt eine zentrale Konfiguration:
`checkstyle.xml`.

- Unterteilung in `Module` und `Eigenschaften`.

- `Module` sind die einzelnen Einheiten die `Checkstyle` ausführt.
`Eigenschaften` beschreiben wie ein `Modul` seine Arbeit erledigen soll.

- (Viele) vordefinierte Module oder komplett eigene.

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

- Wichtige `Eigenschaften`: `baseDir` und `charset`. Es gibt noch viele
[weitere](http://checkstyle.sourceforge.net/config.html#Checker).

Das `TreeWalker`-Modul erstellt einen Syntaxbaum für jede Datei. Hier finden
die meisten Checks statt.

- Jedes Untermodul wird aufgerufen wenn ein definiertes Token gefunden wird.

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
]

---

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
  ## Wie funktioniert das?
]

.right-column[
Module können gewichtet werden. Dazu dient die `Eigenschaft` `severity`.

```xml
<module name="Translation">
    <property name="severity" value="warning"/>
</module>
```

Außerdem können eigene Nachrichten angegeben werden, die bei einem Verstoß
angezeigt werden.

```xml
<module name="MemberName">
    <message key="name.invalidPattern"
             value="Member ''{0}'' is wrong!" />
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
Komplett eigene Module, die nicht auf tokens basieren, können auch angelegt
werden. Es wird eine
[RegEx](https://de.wikipedia.org/wiki/Regul%C3%A4rer_Ausdruck) zum Finden
spezifiziert.

```xml
<!-- Findet Floats die nicht dem Format 3f entsprechen. -->
<module name="RegexpSinglelineJava">
    <property name="format" value="[^\w][\d]+\.f"/>
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
Einzelne Regeln können für ganze Dateien ignoriert werden. Neue Datei:
`checkstyle_suppressions.xml`.

```xml
<suppressions>
    <suppress files="[\w]*Test.java" checks="SomeCheck"/>
</suppressions>
```

Es können auch einzelne Verstöße im Code ignoriert werden.

```java
@SuppressWarnings("checkstyle:nestedifdepth")
public void someMethod() {
    // Many nested structures.
}
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

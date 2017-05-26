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

.title-image[![](img/checkstyle-logo.png)]

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
Einzelne Regeln können für ganze Dateien ignoriert werden. Es wird ein
`SuppresionFilter`-Modul eingetragen.

```xml
<module name="SuppressionFilter">
    <property name="file" value="cs_suppressions.xml" />
</module>
```

Neue Datei:
`cs_suppressions.xml`.

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

.left-column[
  ## Was ist das?
  ## Warum brauche ich das?
  ## Wie funktioniert das?
  ## Wie benutze ich das?
]

.right-column[
### Möglichkeit 1: CLI Tool

```bash
java -jar checkstyle.jar -c checkstyle.xml MyClass.java
```

Es gibt zahlreiche Konfigurationsmöglichkeiten:

- `-f` gibt das Ausgabeformat an. Es gibt `plain` und `xml`.
- `-o` gibt an, in welche Datei das Ergebnis geschrieben werden soll.
- `-e` oder `-x` geben Dateien zum Ignorieren an. `-x` ist RegEx basiert.

.center[![](img/checkstyle-terminal.png)]

Es gibt zwei bekannte Vorgabe-Konfigurationen:
- [Google](http://checkstyle.sourceforge.net/google_style.html)
- [Sun](http://checkstyle.sourceforge.net/sun_style.html)

In den meisten Fällen können diese als Ausgangskonfiguration genommen werden und
bei Bedarf modifiziert werden.
]

---

<!-- class: center, middle -->
# Demo

##### Checkstyle

---

<!-- class: center, middle -->

.title-image[![](img/findbugs-logo.png)]

---

<!-- class: center, middle -->
# Demo

##### FindBugs

---

<!-- class: center, middle -->

.title-image[![](img/jacoco-logo.png)]

---

<!-- class: center, middle -->
# Demo

##### JaCoCo

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

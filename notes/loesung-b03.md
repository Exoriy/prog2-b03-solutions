# Lösung zu Blatt 03: Methodenrefs, Lambdas, Observer

## Überblick

Dieses Dokument enthält meine schriftlichen Notizen und Erklärungen zu Blatt 03.

Die Bearbeitung besteht aus zwei Teilen:

1. Calculator: Anonyme Klassen und Lambda-Ausdrücke
2. LockSnake: Lambda-Ausdrücke, Methodenreferenzen und Observer-Pattern

## Repositories

Calculator:

`https://github.com/Exoriy/prog2-b03-calculator`

LockSnake:

`https://github.com/Exoriy/prog2-b03-locksnake`



---

## 1. Calculator: Anonyme Klassen und Lambda-Ausdrücke

### Aufgabe

Im Calculator-Projekt sollten vier TODO-Stellen in der Methode `setupOperationSelector` bearbeitet werden:

1. Eine neue Operation `Sub` als normale Java-Klasse.
2. Eine neue Operation `Mul` als anonyme Klasse.
3. Eine neue Operation `Div` als Lambda-Ausdruck.
4. Der `ActionListener` der `JComboBox` sollte von einer anonymen Klasse in einen Lambda-Ausdruck umgewandelt werden.

---

### Gradle-Konfiguration

Ich habe das Calculator Projekt als Gradle-Projekt konfiguriert.

Die Datei `settings.gradle` legt den Namen des Gradle-Projekts fest und enthält:

```groovy
rootProject.name = 'prog2-b03-calculator'
```

Die Datei `build.gradle` enthält:

```groovy
plugins {
    id 'java'
    id 'application'
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(25)
    }
}

application {
    mainClass = 'calculator.Main'
}

spotless {
    java {
        googleJavaFormat()
        target 'src/**/*.java'
    }
}
```

Danach habe ich den Gradle Wrapper im Projektordner erzeugt:

```bash
gradle wrapper
```

Das Projekt kann mit Gradle gestartet werden:

```bash
.\gradlew run
```

---

### Klasse `Sub`

Für die Subtraktion habe ich eine eigene Klasse `Sub` erstellt.

```java
package calculator;

/** Simple subtraction. */
public class Sub implements Operation {

  @Override
  public int doOperation(int a, int b) {
    return a - b;
  }
}
```

Diese Klasse implementiert das Interface `Operation`.

In `Calculator.java` wird sie so eingebunden:

```java
operations.put("Sub", new Sub());
```

Hier verwende ich bewusst keine anonyme Klasse und keinen Lambda-Ausdruck, weil die Aufgabe eine normale Java-Klasse verlangt.

---

### Operation `Mul` als anonyme Klasse

Die Multiplikation habe ich als anonyme Klasse umgesetzt:

```java
operations.put(
    "Mul",
    new Operation() {
      @Override
      public int doOperation(int a, int b) {
        return a * b;
      }
    });
```

Eine anonyme Klasse ist hier möglich, weil direkt beim Erzeugen des Objekts die Methode `doOperation` implementiert wird.

---

### Operation `Div` als Lambda-Ausdruck

Die Integerdivision habe ich als Lambda-Ausdruck umgesetzt:

```java
operations.put("Div", (a, b) -> a / b);
```

Das funktioniert, weil Operation ein funktionales Interface ist. Es besitzt nur eine abstrakte Methode:

```java
int doOperation(int a, int b);
```

Deshalb kann Java den Lambda-Ausdruck automatisch als Implementierung von Operation interpretieren.

---

### `ActionListener` als Lambda-Ausdruck

Der ursprüngliche `ActionListener` war als anonyme Klasse geschrieben. Ich habe ihn durch einen Lambda-Ausdruck ersetzt:

```java
operationSelector.addActionListener(
    e -> {
      try {
        result.setText("" + calculate());
      } catch (NumberFormatException ex) {
        System.out.println("Invalid input.");
      }
    });
```

Auch das funktioniert, weil `ActionListener` nur eine zentrale Methode besitzt, nämlich `actionPerformed`.

---

### Lokale Prüfung

Ich habe das Projekt formatiert und geprüft:

```bash
.\gradlew spotlessApply
.\gradlew spotlessCheck
```

Danach habe ich das Projekt gebaut:

```bash
.\gradlew build
```

Außerdem habe ich die Anwendung gestartet:

```bash
.\gradlew run
```

Im Calculator habe ich die Operationen `Add`, `Sub`, `Mul` und `Div` manuell getestet.

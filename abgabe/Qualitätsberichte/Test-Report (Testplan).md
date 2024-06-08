# Testplan

In diesem Dokument sind alle relevanten Informationen der Tests für das Projekt BabyPort zuammengefasst. Dabei werden alle relevanten Testarten, ihre Automatierung, Auswertung, Verwaltung, Metriken, Tools und Konventionen behandelt.
Alle Metriken nach Projektabschluss können im Kapitel [Metriken](./Metriken.md) gefunden werden.

## Welche Arten von Tests werden eingesetzt?

Die aktuelle Teststruktur sieht vollautomatisierte Unit- und Integrationstests vor. Zusätzlich werden manuelle E2E (End-to-End)-Tests sowie API-Tests für die MQTT-Anbindung durchgeführt. Zum besseren Verständnis der genannten Testarten folgen deren Definitionen. Unit- und Integrationstests werden nach der Definition von [Fowler](https://martinfowler.com/articles/practical-test-pyramid.html) durchgeführt, der sich auf eine der bekanntesten Publikationen, [The Test Pyramid von Mike John](https://www.mountaingoatsoftware.com/blog/the-forgotten-layer-of-the-test-automation-pyramid), bezieht.

### Unit-Tests
Unit-Tests stellen sicher, dass eine bestimmte ``Unit`` der Codebasis wie erwartet funktioniert. Je nach Programmiersprache und -paradigma kann sich die Definition einer ``Unit`` unterscheiden. Für unser Projekt im Rahmen der objektorientierten Programmiersprache Java bedeutet ``Unit`` ein logischer Zusammenschluss von Methoden einer Klasse bis hin zu einer ganzen Klasse. 

### Integration-Tests
Integration-Tests beziehen sich nicht wie Unit-Tests auf mehrere logisch zusammenhängende Methoden, sondern testen mehrere Codekomponenten und stellen deren korrekte Zusammenarbeit sicher. Im Rahmen dieses Projekts bedeutet dies in der Regel, dass mehrere Klassen aus mehrere Java Packages getestet werden.

---

Neben diesen beiden Testarten, die automatisiert durchgeführt werden, kommen auch manuelle E2E- und API-Tests zum Einsatz. Ziel dieser ist es, große Teile der Anwendung in Kombination aus Server-Client und Agent, zu testen. 

Bei E2E-Tests werden per Button-Clicks über die Swing UI Events ausgelöst, die über die MQTT-Schnittstelle und den Broker an den Agent weitergeleitet werden. Der Agent führt je nach Anfrage die passenden Docker-Befehle aus. Dabei werden auch Events vom Agent zurück an den Server-Client gesendet. Um diese E2E-Tests manuell durchzuführen, muss zunächst ein lokaler [MQTT-Broker](https://mosquitto.org/) gestartet werden, bei dem sich im Anschluss sowohl Agent als auch ServerClient beim Start verbinden.

Manuelle API-Tests verfolgen ein ähnliches Ziel wie die E2E-Tests, jedoch muss dafür in der Regel nur eine Anwendung, Server-Client oder Agent, gestartet und damit getestet werden. Der Fokus liegt darauf, dass bei ausgelösten Eingaben, die gewünschten Ergebnisse über MQTT gesendet oder die richtigen Docker-Befehle ausgeführt werden. 

## Was ist der Zielwert für die Testabdeckung?

Unser fester Zielwert für die Testabdeckung in Bezug auf die Coverage ist **80%** aller Zeilen.

Zusätzlich haben wir weitere Metriken, die über die Testabdeckung hinausgehen, um die Qualität unserer Software zu überprüfen. Das Quality Gate unterscheidet zwischen New Code und All Code. Für New Code verwenden wir die Einstellung, dass jeder neue Push in GitLab als New Code analysiert wird. Dies hat den Hintergrund, dass bei jeder gepushten Codeänderung die Qualitätsanforderungen eingehalten werden sollen.

Die folgenden Metriken sind Teil unseres Quality Gates und müssen von unserer Software erfüllt werden: 

>![Quality Gate von Sonarqube](/pictures/testplan/quality-gate_sonarqube.png)<br> *Auszug aus dem Sonarqube Quality Gate*

## Welche automatischen Testwerkzeug werden genutzt?

- SonarQube (statische Code-Analyse)
- JUnit Jupiter 5 mit JUnit Params zur Parametrisierung
- Mockito
- Jacoco (Coverage Report in **XML**)


## Wie werden Testfälle verwaltet?

Alle Testfälle werden vollautomatisch über eine CI/CD Pipeline ausgeführt, so dass nachvollzogen werden kann, welcher Testfall in welcher Codeversion fehlgeschlagen ist und warum. Die Testausführung in der Pipeline ist in mehrere Schritte unterteilt. Zuerst werden alle Unit-Tests ausgeführt, danach alle Integrationstests. Für den Code Coverage Report mit Jacoco werden Unit- und Integrationstests zusammen ausgeführt.

Hier ein Beispiel für die Nachvollziehbarkeit, wenn ein Test fehlschlägt:

>![jenkins-logout](/pictures/testplan/jenkins-test-nachvollziehbarkeit.png)<br> *Auszug aus Jenkins*

>![server](/pictures/Tests-Server.png)<br> *Auszug aus den Tests im Server*

>![agent](/pictures/Tests-Agent.png)<br> *Auszug aus den Tests im Agent*

---

## Test Konventionen

- Unit-Test-Klassen folgen der folgenden Konvention: 
  - **\<Klassenname>Test.java**
- Integration-Test-Klassen folgen der folgenden Konvention:
  - **\<Klassenname>IT.java**
- Einzelne Test-Cases folgen der folgenden Konvention:
  - Testnamen: **\<Action>Should\<Expected Result>**
    - z. B.: **dockerStateOnInitShouldBeCreated**
    - Tests die Exceptions testen: **\<Action>ShouldThrow\<Expected Exception Name>Exception**
    - Generell folgen die Testnamen der Java üblichen **Camel Case** Namenskonvention für Methoden
  - Mindestens ein ``assert`` pro Testmethode
  - Keine statischen Importe, da wir nicht davon ausgehen wollen, dass alle Personen, die jemals am Projekt arbeiten werden, alle Frameworks genau kennen. Durch die Vermeidung von statischen Imports wird die Lesbarkeit für diese Personengruppen verbessert und somit das Onboarding erleichtert. 
    - z.B.: anstelle von
      -  Statisch: **when(command.noErrorOccurred()).thenReturn(false);** 
      -  Nicht statisch: **Mockito.when(command.noErrorOccurred()).thenReturn(false);**
    - Dies erhöht die Lesbarkeit und Wartbarkeit
  - Tests-Cases ab **15** Zeilen sollen zur besseren Lesbarkeit und Wartbarkeit unterteilt werden: 
    - Setup - Aufsetzen der Variablen
    - Action - Ausführen der notwendigen Aktionen
    - Assertion - Überprüfung des erwartetem Status der Applikation
  
# UI Testing Dokumentation

Dieses Dokument dokumentiert alle manuellen UI - Testfälle, die verwendet werden, um die gesamte Anwendung hinsichtlich aller Komponenten im Zusammenspiel zu testen.

## Test-Cases
In diesem Abschnitt werden alle manuellen UI - Testfälle dokumentiert.

### Erstellung eines Interfaces (Agents)

1. Klicken auf create
>![create](/pictures/testplan/ui-testplan/testcase-1/server-1_create.png)

2. Editieren des Interfaces mit Beispieldaten
>![edit](/pictures/testplan/ui-testplan/testcase-1/server-1_edit.png)

3. Speichern des Interfaces
>![save](/pictures/testplan/ui-testplan/testcase-1/server-1_save.png)

<u>**Akzeptanzkritieren des Tests:**</u>
- Ein Interface mit dem angegebenen Namen und der Interface-ID muss in der Liste vorhanden sein.
- Nach einem Neustart der Applikation muss das Interface immer noch in der Liste vorhanden sein.
- Das Interface muss sich im Zustand "disconnected" befinden.

### Speichern der Agent-Config eines Interfaces

1. Klicken auf ein Interface
>![click](/pictures/testplan/ui-testplan/testcase-2/agent-config-2_interface.png)

2. Klicken auf File -> Save config
>![save](/pictures/testplan/ui-testplan/testcase-2/agent-config-2_file.png)

<u>**Akzeptanzkritieren des Tests:**</u>
- Prüfung, ob die Datei existiert.
- Prüfung, ob die Werte in der Datei mit den Werten in der Benutzeroberfläche übereinstimmen.

# Testplan E2E - manuelle Ausführung

Im Rahmen dieses Projektes werden manuelle End-To-End (E2E) Tests ausgeführt. Dieses Dokument dient zur Dokumentation dieser Tests

## Vorwort 
Für die Ausführung der manuellen E2E Tests werden folgende Technologien benötigt:
- [Docker](https://www.docker.com/)
- [Mosquitto als MQTT-Broker](https://mosquitto.org/)
- [Maven](https://maven.apache.org/)
- Java 17

## 1 - Starten und Stoppen eines Containers
1. Starte den MQTT Broker Mosquitto mit ``./mosquitto -v`` im verbose Modus
2. Starte den ServerClient mit folgenden Optionen:
   1. Durch die IDE 
   2. Oder manuelles packagen mit ``mvn package -DskipUnitTests`` und direktes ausführen der `jar` mit ``java -jar ./target/babyport-0.0.1-SNAPSHOT-jar-with-dependencies.jar``
3. Erstellung einer neuer Schnittstelle durch das Drücken des `create` Knopfes
4. Um die Schnittstellendaten zu editieren wird der `edit` Knopf gedrückt
5. Dann werden die Daten für die Schnittstelle eingetragen:
   1. Interface id: ``mysql-localhost``
   2. MQTT broker ip: ``127.0.0.1``
   3. MQTT broker port: ``1883``
   4. Docker name: ``mysql-test``
   5. Container start parameters bleibt leer
   6. Image name: ``mysql``
6. Um die Daten zu übernehmen wird der ``save`` Knopf gedrückt
7. Über das Menu `File > Save config` wird die Konfiguration für den Agent in einer ``.yaml`` Datei gespeichert. Diese soll ``agent-configuration.yaml`` genannt werden und in den selben Ordner wie das Agent Projekt gespeichert werden.
   Die Datei sollte follgendes beinhalten:
```yaml
---
mqttBrokerIP: "127.0.0.1"
port: 1883
id: "mysql-localhost"
```
8. Agent starten (wie ServerClient)
9. In der UI den ``connect`` Knopf drücken, um sich mit dem Broker zu verbinden
10. Der aktuelle Status sollte dann ``[connected]`` sein
11. Dann kann mit dem knopfdruch auf ``run`` der Docker Container gestartet werden
12. Das sollte den Docker Container starten
13. Dann soll der Docker Container wieder gestoppt werden durch den knopfdruck auf ``Stop``
14. Das sollte den Docker Container gestoppt haben

## 2 - Zustandsänderungen im Agent erzeugen Status Updates im Server
1. Starte den MQTT Broker Mosquitto mit ``./mosquitto -v`` im verbose Modus
2. Starte den ServerClient mit folgenden Optionen:
   1. Durch die IDE 
   2. Oder manuelles packagen mit ``mvn package -DskipUnitTests`` und direktes ausführen der `jar` mit ``java -jar ./target/babyport-0.0.1-SNAPSHOT-jar-with-dependencies.jar``
3. Erstellung einer neuer Schnittstelle durch das Drücken des `create` Knopfes
4. Um die Schnittstellendaten zu editieren wird der `edit` Knopf gedrückt
5. Dann werden die Daten für die Schnittstelle eingetragen:
   1. Interface id: ``Server1``
   2. MQTT broker ip: ``127.0.0.1``
   3. MQTT broker port: ``1883``
   4. Docker name: ``selenium``
   5. Container start parameters: ``-p 4444:4444 -p 7900:7900 --shm-size="2g"``
   6. Image name: ``selenium/standalone-firefox:4.21.0-20240522``
6. Um die Daten zu übernehmen wird der ``save`` Knopf gedrückt
7. Über das Menu `File > Save config` wird die Konfiguration für den Agent in einer ``.yaml`` Datei gespeichert. Diese soll ``agent-configuration.yaml`` genannt werden und in den selben Ordner wie das Agent Projekt gespeichert werden.
   Die Datei sollte follgendes beinhalten:
```yaml
---
mqttBrokerIP: "127.0.0.1"
port: 1883
id: "Server1"
```
8. Agent starten (wie ServerClient)
9. In der UI den ``connect`` Knopf drücken, um sich mit dem Broker zu verbinden
10. Der aktuelle Status sollte dann ``[connected]`` sein
11. Dann kann mit dem knopfdruch auf ``run`` der Docker Container gestartet werden
12. Das sollte den Docker Container starten
13. Der Status im DockerContainer sollte nun grün im Fenster angezeigt werden
14. Wenn nun der Container mit einem ``docker stop selenium`` auf dem Gerät des Agentes beendet wird, aktualisiert sich der Status und wird Schwarz angezeigt

## 3 - Zustandsänderungen im Server und Agenten durch MQTT-Broker im laufenden Betrieb herunterfahren
1. Starte den MQTT Broker Mosquitto mit ``./mosquitto -v`` im verbose Modus
2. Starte den ServerClient mit folgenden Optionen:
   1. Durch die IDE 
   2. Oder manuelles packagen mit ``mvn package -DskipUnitTests`` und direktes ausführen der `jar` mit ``java -jar ./target/babyport-0.0.1-SNAPSHOT-jar-with-dependencies.jar``
3. Erstellung einer neuer Schnittstelle durch das Drücken des `create` Knopfes
4. Um die Schnittstellendaten zu editieren wird der `edit` Knopf gedrückt
5. Dann werden die Daten für die Schnittstelle eingetragen:
   1. Interface id: ``Server1``
   2. MQTT broker ip: ``127.0.0.1``
   3. MQTT broker port: ``1883``
   4. Docker name: ``selenium``
   5. Container start parameters: ``-p 4444:4444 -p 7900:7900 --shm-size="2g"``
   6. Image name: ``selenium/standalone-firefox:4.21.0-20240522``
6. Um die Daten zu übernehmen wird der ``save`` Knopf gedrückt
7. Über das Menu `File > Save config` wird die Konfiguration für den Agent in einer ``.yaml`` Datei gespeichert. Diese soll ``agent-configuration.yaml`` genannt werden und in den selben Ordner wie das Agent Projekt gespeichert werden.
   Die Datei sollte follgendes beinhalten:
```yaml
---
mqttBrokerIP: "127.0.0.1"
port: 1883
id: "Server1"
```
8. Agent starten (wie ServerClient)
9. In der UI den ``connect`` Knopf drücken, um sich mit dem Broker zu verbinden
10. Der aktuelle Status sollte dann ``[connected]`` sein
11. Dann kann mit dem knopfdruch auf ``run`` der Docker Container gestartet werden
12. Das sollte den Docker Container starten
13. Der Status im DockerContainer sollte nun grün im Fenster angezeigt werden
14. Wenn der Broker nun im laufenden Betrieb herunter gefahren wird, zeigt der ServerClient den Status ``[error]`` an. Der Agent fährt sich in diesem Fall selbständig herunter
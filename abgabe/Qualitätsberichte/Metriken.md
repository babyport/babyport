# Metriken
Die Software entspricht unseren Qualitätsstandards, wenn alle Metriken, die unser Quality Gate abdeckt, erfüllt sind. Wir unterscheiden hier zwischen Pflicht und optionalen Metriken.

## Pflicht-Metriken
Da wir von Anfang an eine Sonarqube-Instanz unseren Code statisch analysieren lassen, haben wir uns für drei Metriken von Sonarqube entschieden. Diese Metriken müssen dann auch für die Endversion erfüllt sein. Wir haben uns für die Metriken von Sonarqube entschieden, da wir diese sehr einfach verfolgen können.
Wir haben uns für die folgenden Metriken entschieden:
- Sicherheit (Security) mit folgendem Wertebereich [A (bestes) — F (schlechtestes)]
- Zuverlässigkeit (Reliability) mit folgendem Wertebereich [A (bestes) — F (schlechtestes)]
- Wartbarkeit (Maintainability) mit folgendem Wertebereich [A (bestes) — F (schlechtestes)]
Die genauere Definition der Metriken kann hier gefunden werden: https://docs.sonarsource.com/sonarqube/latest/user-guide/metric-definitions/

>![Quality-Metrics](/pictures/metrics_sonarqube.png)<br>*Sonarqube-Messungen zum Zeitpunkt der Abgabe für beide Komponenten*

## Tests

In den folgenden zwei Screenshots sieht man das alle Tests erfolgreich durchlaufen.

>![server](/pictures/Tests-Server.png)<br> *Tests im Server*

>![agent](/pictures/Tests-Agent.png)<br> *Tests im Agent*

### Metriken bzgl. der Tests
---
Hier werden alle Daten zu den Prüfungen aufgeschlüsselt.


<u>**Info:**</u> Die durchschnittliche Anzahl der fehlgeschlagenen Tests wurde aus der Anzahl der fehlgeschlagenen Pipeline-Steps berechnet, d.h. Fehlschläge / Summe aller Pipeline-Läufe = durchschnittliche Anzahl der fehlgeschlagenen Tests.

|                                                                                | Server | Agent |
|:------------------------------------------------------------------------------:|:------:|:-----:|
|                              Gesamtmenge der Tests                             |    82    |  68     |
|                 Coverage Insgesamt (Errechnet durch SonarQube)                 |   81,8%     |  83,0%     |
| Durchschnittliche Anzahl fehlgeschlagener Tests (Geschätzt über Pipeline Runs) |   5,91%     |  10,79%     |
|                         Menge der Unit-Tests                                   |   80     |   68    |
|                 Coverage (Unit-Tests) (Errechnet durch IntellJ)                |   70% (Line-Coverage)    |   86% (Line-Coverage)   |
|                      Menge der Integration Tests                               |   2     |    0   |
|             Coverage (Integration-Tests) (Errechnet durch IntelliJ)            |    40%    |     0%  |



## Optionale-Metriken
Zusätzlich zu den drei Pflicht-Metriken für die wir uns während der Entwicklungszeit entschieden haben, erfüllen wir zusätzlich noch die folgenden Metriken:

>![Quality-Gate](/pictures/testplan/quality-gate_sonarqube.png)<br>*Auszug aus dem Sonar Quality Gate*


### Anmerkung zum Einsatz der Metriken
Wenn diese Metriken erreicht werden, gibt uns das den notwendigen Indikator, keine groben Fehler in Bezug auf Sicherheit, Zuverlässigkeit und Wartbarkeit gemacht zu haben. Dennoch verstehen wir diese Metriken nicht als 100%ige Garantie, dass die Software in all diesen Punkten unfehlbar ist.
Daher versuchen wir immer, Metriken so einzusetzen, dass sie die Softwarequalität verbessern, aber wir folgen ihnen nicht immer blind, wenn sie die Software verschlechtern, sei es die Wartbarkeit oder die Lesbarkeit des Codes.

## Security Analysis (Snyk)
Wir verwenden Snyk um unser Projekt zusätzlich zu SonarQube auf potentielle Sicherheitsschwachstellen zu scannen. Dies machen wir über eine zusätzliche Pipeline, die jeden Tag um 05:00 Uhr beide Komponenten auf potentielle Sicherheitsschwachstellen scannt und den Report als Artefakt sowohl in Snyk in der Cloud als auch als Artefakt in Jenkins ablegt.

>![Snyk-Server](/pictures/Snyk/snyk_report_server.png)<br>*Snyk Report des Servers*

>![Snyk-Agent](/pictures/Snyk/snyk_report_agent.png)<br>*Snyk Report des Agents*



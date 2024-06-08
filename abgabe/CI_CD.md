# AUFBAU UND EINSATZ VON CI/CD
Unsere CI/CD Pipeline besteht aus verschiedenen Phasen, die am Ende der Pipeline zu einem fertigen Paket führen. Alle Komponenten, die von uns geschrieben werden, müssen diese Schritte der Pipeline bei jedem Push in Main oder bei der Erstellung eines Merge-Requests erfolgreich durchlaufen. Wenn der Code diese Schritte nicht erfolgreich durchlaufen kann, wird er nicht akzeptiert. Sobald eine Pipeline Stufe nicht erfolgreich ist, wird der Durchlauf abgebrochen und ist fehlerhaft. Alle unsere Pipelines sind in Code geschrieben, so dass alle Änderungen ständig in Git (https://github.com/babyport/babyport/tree/main/pipelines) verfolgt werden.

| Kategorie                                   | Pipeline Stufe            | Zweck                                                                                                                                                                           |
| ------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Quellcodeverwaltung                         | GIT CHECKOUT main         | In diesem Schritt wird der HEAD von main ausgecheckt, ausgelöst durch einen Push oder Merge Request.                                                                            |
| Testing                                     | RUN UNIT TESTS            | In diesem Schritt werden die Unit-Tests der jeweiligen Komponente ausgeführt.                                                                                                   |
|                                             | RUN INTEGRATION TESTS     | In diesem Schritt werden die Integrationstests der jeweiligen Komponente durchgeführt.                                                                                          |
| Statische Code-Analyse                      | RUN STATISCHE CODEANALYSE | In diesem Schritt wird eine SonarQube-Analyse des Quellcodes durchgeführt. Außerdem wird die Code Coverage berechnet und als JaCoCo Bericht an SonarQube übergeben.             |
| Packaging der Applikation                   | PACKAGE APPLICATION       | In diesem Schritt wird die Anwendung mit Hilfe des Maven Package Goals gepackt.<br>                                                                                             |
| Auslieferung in die dev-Umgebung            | DELIVERY TO DEV           | In diesem Schritt wird die gepackte JAR aus dem vorherigen Schritt für manuelle End-to-End-Tests an die Sandbox übergeben.                                                      |
| <br>Auslieferung in die produktion-Umgebung | DELIVERY TO PRODUCTION    | In diesem Schritt wird geprüft, ob die Anwendung in der Entwicklungsumgebung funktioniert. Wenn dies der Fall ist, wird die Komponente in die Produktionsumgebung ausgeliefert. |

Die folgende Abbildung zeigt schematisch, in welcher Reihenfolge die Pipeline durchlaufen werden muss, um eine gepackte JAR-Applikation in der Sandbox zu deployen. Darüber hinaus kann die gepackte JAR-Applikation auch in der Produktion deployed werden, wenn das Deployment in der Sandbox erfolgreich war.

>![Pipeline](/pictures/pipeline.png)<br> *Eine grafische Darstellung der Pipeline Steps*
## Stages
Unser Setup sieht zwei Stages vor. Wir verwenden eine dev und eine prod Stage. Das bedeutet, dass alle Schritte der Pipeline auf der dev Umgebung ausgeführt werden. Sobald die Healthchecks für die dev Umgebung in Ordnung sind, wird die prod Pipeline getriggert, die dann die Applikation baut und an die prod Umgebung ausliefert.

## Pipeline Status
Um die Pipeline noch besser in den Workflow zu integrieren, wird der Status der Pipeline an GitLab gespiegelt. Das bedeutet, dass der Entwickler direkt im Merge Request sehen kann, ob die Pipeline durchlaufen wird oder nicht, wobei der Merge solange blockiert wird, bis die Pipeline wieder grün ist.

>![Pipeline-Status](/pictures/Pipeline-Status.png) <br> *Auszug aus GitLab mit dem Pipeline Status*

## Weitere Pipelines
Zusätzlich zu der Standard Pipeline werden weitere Pipelines regelmäßig ausgeführt.

### Renovate
Eine Renovate-Instanz wird stündlich durch eine Pipeline mit Timer gestartet und überprüft jede Abhängigkeit in der pom.xml beider Projekte auf neue Versionen der Abhängigkeiten und veröffentlicht dann einen Merge-Request in diesen Projekten, so dass das Team entscheiden kann, ob wir diese Aktualisierung durchführen oder weitere Tests mit dieser Aktualisierung durchführen müssen, um sie in die Software zu integrieren.

>![Renovate](/pictures/Renovate.png) <br> *Auszug aus Renovate Merge Request*

### Synk
Jeden Tag wird eine neue Pipeline gestartet, die beide Komponenten einem kompletten Security Scan unterzieht. Das Ziel ist es herauszufinden, ob es Schwachstellen in unserer Software gibt, die wir beheben müssen.
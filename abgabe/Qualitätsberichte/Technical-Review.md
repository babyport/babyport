# Technical Review

## Metadata
In diesem Abschnitt werden alle Metadaten definiert, die für die Erfassung der Struktur des Reviews erforderlich sind.

| Typ                               | Wert                                       |
| --------------------------------- | ------------------------------------------ |
| Datum des Reviews (in DD.MM.YYYY) | 01.06.2024                                 |
| Startzeitpunkt (in HH:MM)         | 15:00                                      |
| Endzeitpunkt (in HH:MM)           | 16:15                                      |
| Teilnehmer                        | Paul Bader, Marius Wörfel, Sarah Ficht     |
| Moderator                         | Marius Wörfel                              |
| Protokollant                      | Sarah Ficht                                |
| Reviewer                          | Paul Bader                                 |
| Review Methodik                   | Code Review                                |

## Kritierien für das Review der Komponenten
1. Struktur des Codes (Packagestruktur)
2. SOLID
3. DRY
4. Testbarkeit
5. Sind wichtige Logiken nicht getestet?
6. Lesbarkeit innerhalb einer Datei
   1. Variablennamen
   2. Methodennamen
   3. Klassennamen
   4. Struktur
7. Code Encapsulation
8. Code Style (z.B. Formatierung) & Conventions allgemeingültig
9. Error Handling
10. Daten Strukturen
11. Code Duplikationen
12. Wartbarkeit 
13. Skalierbarkeit
14. Security

## Liste aller Komponenten in Review (Ziel / Fokus)
Die Liste soll in folgendem Format erstellt werden: (Name der Komponente, Pfad zur Komponente innerhalb des Projektes, Begründung für das Review dieser Komponente).

**Format:** - Komponente, /package/package/name.class, Weil dies Komponente maßgeblich für die Funktionalität verantwortlich ist

### Agent (Sidecar) Komponente
---
- MQTTAgentWrapper, src/communication/MQTTAgentWrapper, Diese Komponente ist maßgeblich für die Kommunikation verantwortlich und daher für den Funktionsumfang sehr relevant.
- CommandController, src/command/CommandController, Diese Komponente steuert den Großteil der Logik für die Befehlsausführung und ist daher für den Funktionsumfang sehr relevant.

### Server-Client Komponente
---
- MQTTClientWrapper, src/communication/MQTTClientWrapper, Diese Komponente ist maßgeblich für die Kommunikation verantwortlich und daher für den Funktionsumfang sehr relevant.
- Controller, src/controller/Controller, Diese Komponente steuert den Großteil der Logik und ist daher für den Funktionsumfang sehr relevant.

## Ergebnisse
In diesem Abschnitt werden alle Anmerkungen aus dem Review festgehalten, so dass das Team relevante Informationen und Verbesserungsvorschläge aus dem Review ableiten kann.

### Legende
Die Zeilen, in denen die Bemerkung nicht eingetragen wurde, haben keine positive oder negative Bemerkung erhalten.

### Review MQTTAgentWrapper (Agent)

| Kriterium                                                    | Code-line | Bemerkung (dokumentiert wie gesprochen (wortgetreu))                                              |     |
| ------------------------------------------------------------ | --------- | --------------------------------------------------- | --- |
| Struktur des Codes (Packagestruktur)                         | l. xx-xx  | ----------------------------------------            |     |
| SOLID                                                        | l. xx-xx  | ----------------------------------------            |     |
| DRY                                                          | l. xx-xx  | ----------------------------------------            |     |
| Testbarkeit                                                  | l. xx-xx  | ----------------------------------------            |     |
| Sind wichtige Logiken nicht getestet?                        | l. xx-xx  | ----------------------------------------            |     |
| Lesbarkeit innerhalb einer Datei:                            |           | Kommentare sind mehr (als bei ServerClient) und gut |     |
| Variablennamen                                               | l. xx-xx  | ----------------------------------------            |     |
| Methodennamen                                                | l. xx-xx  | ----------------------------------------            |     |
| Klassennamen                                                 | l. xx-xx  | ----------------------------------------            |     |
| Struktur                                                     | l. xx-xx  | ----------------------------------------            |     |
| Code Encapsulation                                           | l. xx-xx  | ----------------------------------------            |     |
| Code Style (z.B. Formatierung) & Conventions allgemeingültig | l. xx-xx  | ----------------------------------------            |     |
| Error Handling                                               | l. 70+    | Throws in Methodensignatur fehlt                          |     |
| Daten Strukturen                                             | l. xx-xx  | ----------------------------------------            |     |
| Code Duplikationen                                           | l. xx-xx  | ----------------------------------------            |     |
| Wartbarkeit                                                  | l. xx-xx  | ----------------------------------------            |     |
| Skalierbarkeit                                               | l. xx-xx  | ----------------------------------------            |     |
| Security                                                     | l. xx-xx  | ----------------------------------------            |     |

---

### Review CommandController (Agent)

| Kriterium                                                    | Code-line | Bemerkung (dokumentiert wie gesprochen (wortgetreu))                             |
| ------------------------------------------------------------ | --------- | ---------------------------------------- |
| Struktur des Codes (Packagestruktur)                         | l. xx-xx  | ---------------------------------------- |
| SOLID                                                        | l. xx-xx  | ---------------------------------------- |
| DRY                                                          | l. xx-xx  | ---------------------------------------- |
| Testbarkeit                                                  | l. xx-xx  | ---------------------------------------- |
| Sind wichtige Logiken nicht getestet?                        | l. xx-xx  | ---------------------------------------- |
| Lesbarkeit innerhalb einer Datei:                            |           | Doku und Kommentare auffallend positiv, da in Menge vorhanden                   |
| Variablennamen                                               | l. xx-xx  | ---------------------------------------- |
| Methodennamen                                                | l. xx-xx  | ---------------------------------------- |
| Klassennamen                                                 | l. xx-xx  | ---------------------------------------- |
| Struktur                                                     | l. xx-xx  | ---------------------------------------- |
| Code Encapsulation                                           | l. xx-xx  | ---------------------------------------- |
| Code Style (z.B. Formatierung) & Conventions allgemeingültig | l. xx-xx  | ---------------------------------------- |
| Error Handling                                               | l. xx-xx  | ---------------------------------------- |
| Daten Strukturen                                             | l. xx-xx  | ---------------------------------------- |
| Code Duplikationen                                           | l. xx-xx  | ---------------------------------------- |
| Wartbarkeit                                                  | l. xx-xx  | ---------------------------------------- |
| Skalierbarkeit                                               | l. xx-xx  | ---------------------------------------- |
| Security                                                     | l. xx-xx  | ---------------------------------------- |

---

### Review MQTTClientWrapper (Server-Client)

| Kriterium                                                    | Code-line    | Bemerkung (dokumentiert wie gesprochen (wortgetreu))                                                                                                                                                                                                                                 |
| ------------------------------------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Struktur des Codes (Packagestruktur)                         | l. 76 - 101  | fragwürdig, warum das `enum EventCommand` in der Klasse drin ist und nicht eine extra Datei ist                                                                                                                                            |
| SOLID                                                        | l. xx-xx     | s: log geschichte könnte in Elternklasse ausgelagert werden / abstrakte klasse<br>s: `getAttributes` und `setAttributes` könnten auch ein Refactoring vertragen. Hier sind die expliziten Attribute alle hardgecoded<br>                   |
| DRY                                                          | l. 415 - 417 | `hashCode` Methodenaufruf gibt direkt an `super` weiter. Braucht man diese Methode wirklich?                                                                                                                                               |
| Testbarkeit                                                  | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Sind wichtige Logiken nicht getestet?                        | l. xx-xx     | wird schon passen (ich schau mir doch die tests nicht an lol)                                                                                                                                                                              |
| Lesbarkeit innerhalb einer Datei:                            |              |                                                                                                                                                                                                                                            |
| Variablennamen                                               | l. 67        | `client` könnte zu `mqttClient` umbenannt werden. Macht den Code im Rest der Klasse lesbarer                                                                                                                                               |
| Methodennamen                                                | l. 225       | `clientNullOrNotConnected()` 2 "Checks" im Methodennamen (Methode macht mehr als eine Sache) -> in 2 Methoden auslagen und besseren Oberbegriff finden (oder einfach `isConnected()` invertieren, weil das genau die umgekehrte Logik ist) |
|                                                              | l. 419       | `hasNoMessageConsumer` -> `hasMessageConsumer` (invertierte Methodennamen sind oft verwirrend)                                                                                                                                             |
| Klassennamen                                                 | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Struktur                                                     | l. xx-xx     | - Konventionen zur Reihenfolge von Feldern und Methoden (zuerst static final, dann static, dann ...)<br>- alle Klassenattribute nach oben und nicht zwischen den Methoden<br>                                                              |
|                                                              |              | beim Constructor sollte die Reihenfolge der Zuweisungen der Reihenfolge der Argumente entsprechen                                                                                                                                          |
|                                                              | l. 274       | dings mit `splitTopic` (Was passiert hier? Nicht einfach lesbar aus Code)                                                                                                                                                                  |
| Code Encapsulation                                           | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Code Style (z.B. Formatierung) & Conventions allgemeingültig | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Error Handling                                               | l. 189 - 191 | bei bereits bestehender Connection wird nur returned. Hier wäre eine explizite Fehlerbehandlung sinnvoller, denke ich                                                                                                                      |
|                                                              | l. 210 - 212 | auch hier wieder Fehler werfen                                                                                                                                                                                                             |
|                                                              | l. 235 - 237 | hier auch                                                                                                                                                                                                                                  |
| Daten Strukturen                                             | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Code Duplikationen                                           | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Wartbarkeit                                                  | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Skalierbarkeit                                               | l. xx-xx     | passt                                                                                                                                                                                                                                      |
| Security                                                     | l. xx-xx     | keine Ahnung                                                                                                                                                                                                                               |

---

### Review Controller (Server-Client)

| Kriterium                                                    | Code-line   | Bemerkung  (dokumentiert wie gesprochen (wortgetreu))                                                                                                                               |
| ------------------------------------------------------------ | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Struktur des Codes (Packagestruktur)                         | l. xx-xx    | `EventCommand` wieder als inneres `enum`                                                                                                  |
| SOLID                                                        | l. xx-xx    | s: die ganzen `@Override` könnten auch abstrakte Klassen sein und keine Interfaces.                                                       |
| DRY                                                          | l. xx-xx    | switch dies das                                                                                                                           |
| Testbarkeit                                                  | l. xx-xx    | für jeden switch case ein Test eher net so geil                                                                                           |
| Sind wichtige Logiken nicht getestet?                        | l. xx-xx    | wird schon                                                                                                                                |
| Lesbarkeit innerhalb einer Datei:                            |             |                                                                                                                                           |
| Variablennamen                                               | l. 61       | `allCasted`??                                                                                                                             |
| Methodennamen                                                | l. xx-xx    | passt                                                                                                                                     |
| Klassennamen                                                 | l. 69       | `de.babyport.ui.MainGui.EventCommand eventCMD = (MainGui.EventCommand)` ????                                                              |
| Struktur                                                     | l. 70 - 124 | könnte man auch in methoden auslagen / refactoren, dass man gar nicht erst einen switch braucht (sollte man sowieso nicht eigentlich :) ) |
| Code Encapsulation                                           | l. xx-xx    | passt                                                                                                                                     |
| Code Style (z.B. Formatierung) & Conventions allgemeingültig | l. xx-xx    | Reihenfolge der Methoden nach Sichtbarkeit                                                                                                |
| Error Handling                                               | l. xx-xx    | gibbet net                                                                                                                                |
| Daten Strukturen                                             | l. xx-xx    | passt                                                                                                                                     |
| Code Duplikationen                                           | l. xx-xx    | `@Override` dings                                                                                                                         |
| Wartbarkeit                                                  | l. xx-xx    | switch ist meh (ein neuer Eventtype, alle switches anpassen.)                                                                             |
| Skalierbarkeit                                               | l. xx-xx    | switch dings                                                                                                                              |
| Security                                                     | l. xx-xx    | passt                                                                                                                                     |

Sonstiges / Wichtigste Anmerkungen
---
Keine JavaDoc in ServerClient (Lesbarkeit schwierig, Abstraktion zu viel und zu früh -> unnoetig viel Aufwand bei Test (mocken))

1. EventCommand (doppelt benamt, andere Aufgabe)
2. Nicht nur Interfaces, auch abstrakte Klasse mit default Verhalten. Va. Swtich Case mit gleichen duplizierten Zeilen
3. Enum weg, stattdessen einzelne Klassen welche von Eltern erben (mit Logik/ default Verhalten).


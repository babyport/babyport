# SonarQube Code Smell Entscheidungen

Manche Code Smells machen nicht immer Sinn für alle Entwickler, daher wollen wir diesem Dokuement alle Entscheidungen dokumentieren, die wir treffen bzgl. Code Smells. Das soll dafür Sorgen das es einen definierten Stand gibt und klar ist welche Entscheidungen getroffen worden sind.

## Entscheidungen

20231206    Code Smells welche "Avoid inline conditionals." enthalten, werden künftig ignoriert.
            <u>**Begründung:**</u> Einzige Begründung für den Code Smells ist die empfundene, schlechtere Lesbarkeit des Codes. Da wir Lazy Eval sowieso nur ohne Nesting von Funktionen nutzen werden wir diesen Code Smells künftig ignorieren.

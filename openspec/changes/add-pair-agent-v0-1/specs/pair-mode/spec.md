# Spec Delta

## Purpose

Der Pair-Modus ermöglicht verständnis-erhaltende Zusammenarbeit mit einem KI-Agenten im Dialog-Takt: geteilter Kontext, dosierte Save-Trigger-Kommentare mit Reveal-Fragen und ein mechanisch erzwungener Lautstärkeregler – statt Auftragstakt und Diff-Abnicken. (Grundlage: SPEC.md §4 und §9, Entscheidungen 1–7 in §8.)

## ADDED Requirements

### Requirement: Pair-Session starten und beenden
Das System SHALL Pair-Sessions per Kommando starten und beenden. Beim Start SHALL es das Markdown-Gedächtnis in den Agent-Kontext laden und ein Session-Protokoll beginnen; nach dem Ende SHALL keine Hintergrundaktivität der Session fortbestehen.

#### Scenario: Session starten
- **WHEN** der Entwickler das Session-Start-Kommando ausführt
- **THEN** wird eine neue Session erzeugt, die Notiz-Datei des Gedächtnisses ist im Agent-Kontext geladen und das Session-Protokoll wird geführt

#### Scenario: Session beenden
- **WHEN** der Entwickler die Session beendet
- **THEN** ist das Session-Protokoll als Markdown-Datei persistiert und die Session ist vollständig abgeschlossen

### Requirement: Geteilter Kontext
Das System SHALL bei jeder Agent-Anfrage den geteilten Kontext des Menschen mitliefern: aktive Datei, aktuelle Selection und Terminal-Output.

#### Scenario: Selection als Kontext
- **WHEN** der Entwickler Code auswählt und dazu eine Frage stellt
- **THEN** enthält der Kontext der Anfrage den ausgewählten Text mit Dateipfad und Zeilenbereich

#### Scenario: Aktive Datei als Kontext
- **WHEN** der Entwickler eine Anfrage ohne expliziten Dateibezug stellt
- **THEN** enthält der Kontext die aktive Datei mit Pfad und Inhalt

#### Scenario: Terminal-Output als Kontext
- **WHEN** der Entwickler sich auf einen Terminal-Ausdruck bezieht
- **THEN** enthält der Kontext den zugehörigen Terminal-Output

### Requirement: Save-Trigger-Kommentare im Kollege-Modus
Im Kollege-Modus SHALL der Agent beim Speichern oder Verlassen einer Code-Region höchstens einen Kommentar abgeben: kurz (2–3 Sätze) im Chat, mit Datei- und Zeilenverweis. Im Still-Modus SHALL auf Save-Trigger kein ungefragter Kommentar folgen, auch wenn Inhalte bemerkenswert sind.

#### Scenario: Kommentar im Kollege-Modus
- **WHEN** der Entwickler im Kollege-Modus speichert und der Agent eine bemerkenswerte Stelle sieht
- **THEN** erscheint im Chat höchstens ein Kommentar von 2–3 Sätzen mit file:line-Verweis

#### Scenario: Still-Modus bleibt still
- **WHEN** der Entwickler im Still-Modus speichert
- **THEN** gibt es keinen ungefragten Kommentar

#### Scenario: Nichts zu sagen
- **WHEN** der Agent beim Save keinen bemerkenswerten Inhalt erkennt
- **THEN** erscheint kein Kommentar

### Requirement: Form nach Inhalt mit aufklappbarer Antwort
Kommentare SHALL nach Inhalt formuliert werden: objektive Fehler als Feststellung; mutmaßliche Design-Schwächen als Frage, deren Antwort zunächst verborgen ist und per Klick aufgedeckt wird (Reveal). Die Antwort SHALL erst nach eigener Überlegung des Menschen sichtbar werden.

#### Scenario: Feststellung bei objektivem Fehler
- **WHEN** der Agent einen objektiven Fehler im gespeicherten Code erkennt
- **THEN** wird der Kommentar als Feststellung mit direkter Begründung formuliert

#### Scenario: Frage mit verborgener Antwort
- **WHEN** der Agent einen Design-Zweifel sieht
- **THEN** wird der Kommentar als Frage formuliert und die Antwort ist erst nach dem Reveal-Klick sichtbar

#### Scenario: Reveal zeigt Antwort
- **WHEN** der Entwickler die verborgene Antwort aufdeckt
- **THEN** wird die Antwort angezeigt und der Klick als Verhaltenssignal protokolliert

### Requirement: Lautstärkeregler als Erlaubnis-Grenze
Das System SHALL einen Lautstärkeregler (Still / Kollege) in der Session-UI anbieten. Der Regler SHALL die Erlaubnis-Grenze setzen: Ungefragte Kommentare nur im Kollege-Modus; ein Wechsel SHALL sofort wirken; kein adaptives Verhalten MAY die Erlaubnis unterlaufen.

#### Scenario: Sofortiges Wirken
- **WHEN** der Entwickler während einer Session auf Still schaltet
- **THEN** gibt der Agent ab diesem Moment keine ungefragten Kommentare mehr ab

#### Scenario: Regler im UI
- **WHEN** eine Pair-Session läuft
- **THEN** ist der Regler in der Session-UI bedienbar, ohne eine Konfigurationsdatei zu editieren

### Requirement: Explizite Fragen immer beantworten
Das System SHALL dem Agenten vorschreiben, explizite Fragen des Menschen in jedem Modus direkt zu beantworten.

#### Scenario: Frage im Still-Modus
- **WHEN** der Entwickler im Still-Modus eine explizite Frage stellt
- **THEN** antwortet der Agent direkt auf die Frage

### Requirement: Vorschläge statt Writes
Das System SHALL Code-Vorschläge des Agenten ausschließlich zur manuellen Übernahme anbieten. Ein automatisches Schreiben von mehrzeiligen Vorschlägen in Dateien SHALL es nicht geben; jede Übernahme ist eine bewusste Handlung des Menschen.

#### Scenario: Übernahme nur manuell
- **WHEN** der Agent mehrzeilige Zeilen vorschlägt
- **THEN** wird nichts automatisch in die Datei geschrieben und die Übernahme erfordert eine ausdrückliche Handlung des Entwicklers

### Requirement: Verhaltenssignal-Protokollierung
Das System SHALL Reveal-Klicks, ausbleibende Reaktionen auf Save-Trigger-Kommentare (nach definiertem Zeitraum) und Moduswechsel mit Zeitstempel ins Session-Protokoll schreiben.

#### Scenario: Moduswechsel protokolliert
- **WHEN** der Entwickler den Lautstärkeregler verändert
- **THEN** erscheint ein Eintrag mit Zeitstempel und Richtung im Session-Protokoll

#### Scenario: Ausbleibende Reaktion protokolliert
- **WHEN** auf einen Save-Trigger-Kommentar innerhalb des definierten Zeitraums weder Reveal-Klick noch Antwort erfolgt
- **THEN** erscheint ein Protokolleintrag über die ausgebliebene Reaktion

### Requirement: Markdown-Gedächtnis
Das System SHALL ein menschenlesbares Markdown-Gedächtnis führen: eine wachsende Notiz-Datei (beim Session-Start geladen) und das Session-Protokoll. Der Mensch MUST beide Dateien direkt lesen und editieren können; Änderungen an der Notiz-Datei SHALL in der nächsten Session wirksam werden.

#### Scenario: Notizen beim Start geladen
- **WHEN** eine Pair-Session startet
- **THEN** stehen die Inhalte der Notiz-Datei im Agent-Kontext zur Verfügung

#### Scenario: Menschliche Bearbeitung wirkt fort
- **WHEN** der Entwickler die Notiz-Datei zwischen zwei Sessions bearbeitet
- **THEN** nutzt die nächste Session die bearbeiteten Notizen

### Requirement: Konfigurierbarer Provider
Das System SHALL Provider und Modell über Konfiguration wählen (provider-neutraler Layer). Fehlende oder ungültige Konfiguration SHALL mit einer verständlichen Meldung abgelehnt werden, ohne still fehlzuschlagen.

#### Scenario: Konfiguriertes Modell wird genutzt
- **WHEN** eine gültige Provider-Konfiguration vorliegt
- **THEN** laufen alle Agent-Anfragen über den konfigurierten Provider und das konfigurierte Modell

#### Scenario: Fehlende Konfiguration
- **WHEN** kein API-Zugang oder Modell konfiguriert ist
- **THEN** meldet das System das verständlich, statt still zu scheitern

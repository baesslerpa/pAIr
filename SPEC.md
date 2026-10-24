# Pair Agent – Konzept

*Ein Agent als echter Pair-Programming-Partner: Dialog, gemeinsames Zeigen, abgestufte Autonomie.*

---

## 1. Ausgangslage

Aktuelle Coding-Agents (Claude Code, Cursor &amp; Co.) sind konsequent auf **Durchsatz** optimiert: Ein Prompt, der Agent läuft los, liefert viel Code am Stück, der Mensch reviewt am Ende einen Diff. Das funktioniert für Geschwindigkeit – aber es erzeugt zwei ernsthafte Probleme:

1. **Verflachung.** Wer den Lösungsweg delegiert, baut kein mentales Modell des Systems mehr auf. Nach Monaten hat man "nur noch eine vage Ahnung, was passiert". Der Lerneffekt und der "Kick" des Verstehens gehen verloren.
2. **Passives Konsumieren.** Review als Diff-Abnicken ist der schlechteste Lernmodus, der sich denken lässt – und gleichzeitig der Standard-Workflow, den die Tools erzwingen.

Die Tools werden in diese Richtung *weiter* entwickeln: mehr Autonomie, weniger Sichtbarkeit, weniger Reibung. Reibung ist für den Hersteller ein Bug – für den Entwickler war sie aber ein Lernmechanismus.

**Die These dieses Konzepts:** Die Lücke "verständnis-erhaltende Zusammenarbeit mit einem Agent" ist derzeit fast unbesetzt. Die Modelle können die nötigen Fähigkeiten bereits – es fehlt der Rahmen, der die richtige Interaktionsform erzwingt.

---

## 2. Leitprinzip

> **Der Agent hat keine Agenda.**

Heutige Agents sind *Vollstrecker*: Sie haben einen Auftrag und wollen ihn beenden. Ein Pair-Partner hat keinen Auftrag. Er ist anwesend, reagiert auf das, was der Mensch tut, und sagt seine Meinung – wenn gefragt oder wenn es sich anbietet.

Das Tool macht den Agenten also nicht mächtiger, sondern demotet ihn: von der *handelnden Entität* zum *anwesenden Kollegen*. Fast jede weitere Entscheidung folgt aus diesem einen Prinzip.

Daraus abgeleitet:

- Der Agent spricht nie ungefragt über längere Strecken.
- Er reagiert auf die Aktionen des Menschen, nicht auf ein eigenes TODO.
- Alle Schreibrechte bleiben beim Menschen (siehe Autonomie-Modi, Abschnitt 5 – dort wird das kontrolliert aufgeweicht).
- Er muss widersprechen *können* – ein Partner, der alles gut findet, ist wertlos.

---

## 3. Die drei Tragflächen

Das Konzept ruht auf drei Säulen. Jede diskutierte Funktion hängt an einer davon.

### 3.1 Dialog

Zusammenarbeit in Dialog-Takt statt in Auftrags-Takt: Der Mensch schreibt ein paar Zeilen, der Agent kommentiert *vorher* ("pass auf, das bricht bei leerem Input") – nicht am Ende als Review. Umgekehrt schlägt der Agent ein paar Zeilen vor, der Mensch sagt "mach aus der Schleife ein early return", der Agent passt an.

Der Dialog selbst ist das Erklärmedium: Begründungen ("warum willst du das hier cachen?") laufen in beide Richtungen und halten das Systemverständnis beim Menschen. Damit löst das Konzept direkt das Ausgangsproblem: Der Widerstand – in Dialogform – kommt zurück.

### 3.2 Zeigen (Deixis)

Pair Programming lebt von Zeigegesten: "*diese* Zeile", "*genau hier* passt das nicht". Chat-Interfaces können das nicht – deshalb fühlen sie sich nicht wie Pair Programming an. Zeigen muss in beide Richtungen funktionieren und im Editor verankert sein:

**Agent → Mensch:**

- **Zeilen-Highlights:** Der Agent markiert Stellen im Code ("das ist der Bug, Zeile 47, weil der Wert hier null sein kann") statt sie nur im Chat zu beschreiben.
- **Gutter-Marker mit Begründung:** Markierte Stellen mit Hover-Erklärung.
- **Was-wäre-wenn-Annotationen:** Der Agent zeigt auf eine Stelle im *aktuellen* Code und sagt: "wenn wir deinen Ansatz von oben weiterbauen, gibt es genau hier ein Problem." Das ist Vorher-Schau statt Nachher-Review – genau dort entsteht gemeinsames Verständnis.

**Mensch → Agent:**

- **Selection als primärer Input:** "Mach hier ein Inline draus" – mit "hier" = das Markierte. Kurze zeigerbasierte Kommentare statt langer Prompts.
- **Pins/Marker:** "Das ist zu kompliziert, reden wir später drüber" – Punkte setzen, ohne den Faden im aktuellen Problem zu verlieren.
- **Freihand-Notizen im Code:** Der Agent versteht diese im Session-Kontext als Kommentar mit Zeigegeste, nicht als isolierte Texteingabe.

Ein zweizeiliger Selection-Kommentar ist ergonomischer als jeder getippte Prompt. Die Editor-Integration ist dafür zwingend – ein Terminal-Tool kann Deixis nicht leisten.

### 3.3 Abgestufte Autonomie

Der Agent *kann* autonom arbeiten – aber Autonomie ist niemals der Default, sondern **das Ergebnis eines Dialogs**. Der Mensch sagt: "Ok, mach den Rest alleine." Damit bleibt die Kontrolle über den Modus beim Menschen, und der Übergang ist sauber geregelt (Details in Abschnitt 5).

---

## 4. Interaktionsmodell: Der Pair-Modus

Der Grundmodus ist der enge Dialog am gemeinsamen Code. Wichtige Regeln:

**Trigger für ungefragte Kommentare** (der kritischste Punkt – falsch dosiert wird sowas unbrauchbar wie ein Klippy):

1. Der Mensch speichert oder verlässt eine Code-Region.
2. Der Mensch fragt explizit ("guckst du mal drüber?").

**Dosierung – der Lautstärkeregler:**

- *Still:* Agent antwortet nur auf explizite Fragen.
- *Bei Fragen:* Agent kommentiert nur auf Trigger, die man selbst auslöst.
- *Kollege:* Voller Pair-Modus – proaktive Kommentare im Minuten-Takt.

Die Stufen unterscheiden sich im Trigger-Vokabular: *Still* = direkte Fragen; *Bei Fragen* = zusätzlich Trigger, die der Mensch selbst auslöst (Save, Region verlassen); *Kollege* = zusätzlich echte Initiative des Agenten. V0.1-Kollege ist damit de facto "Bei Fragen" – eigene Agenten-Initiative kommt später.

**Erlaubnis und Dosierung als zwei Ebenen.** Der Regler bestimmt, was der Agent *darf* – eine harte, mechanisch erzwungene Grenze. Was er daraus im Einzelfall *tut*, entscheidet ein **Proaktivitäts-Gate**: eine zentrale Stelle im Code, die bei jedem Trigger wählt – schweigen oder sprechen, in welcher Form, in welchem Umfang. Die Invarianten garantiert der Code (in Still niemals ungefragt, max. ein Kommentar pro Save); das Urteil "lohnt sich das hier?" und die Form trifft das Modell innerhalb eines Budgets. So bleibt der Regler ehrlich: Kein adaptives Verhalten kann die Erlaubnis unterlaufen. Die geplante Kontingenz (CoMeT-Mechanik) verändert nur Parameter dieses Gates, keine neuen Codepfade. Ausbau in Stufen: erst statisches Gate mit Logging der Verhaltenssignale (V0.1), dann Cooldown (N Saves Ruhe nach Interaktion), dann kontingente Leiter innerhalb des Kollege-Modus – mit asymmetrischer Dosierung: schnell runter (Genervt-Sein killt die freiwillige Nutzung), langsam rauf.

Der Kollege-Modus ist ein Zustand für ein paar Stunden am Tag, nicht für acht. Der Regler gehört ins UI, nicht in eine Config-Datei.

**Geteilter Kontext – erweitert per Zeigegeste:** Basis-Kontext ist das, was der Mensch gerade sieht: offene Datei, Selection, Terminal-Output, letzte Änderungen. Darüber hinaus *darf* der Agent weitere Dateien lesen – wie ein Kollege mit eigenem Laptop, der kurz im Code stöbert. Der entscheidende Unterschied zu heutigen Agent-Tools ist nicht das Lesen, sondern der **Zugang zum Kontext**: Der Agent kann nicht still in einem Wissensraum arbeiten, den der Mensch nie sieht. Was er woanders findet, bringt er aktiv in den gemeinsamen Raum – "guck mal da noch" – mit Sprung zur Stelle und Begründung, warum das relevant ist. Neuer Kontext entsteht also immer per Zeigegeste in beide Richtungen, nie als stille Hintergrund-Erweiterung. Kein "Agent hat 12 Dateien gelesen", während der Mensch tippt – sondern "Agent hat in 12 Dateien gelesen und zeigt dir die 2 Stellen, auf die es ankommt".

**Vorschläge statt Writes:** Der Agent schlägt Zeilen vor (Inline-Suggestion oder Chat im Copy-Paste-Format). Der Mensch übernimmt manuell oder per explizitem Befehl. Kein Auto-Apply für Mehrzeiler. Das erzwingt genau das Lesen, das im reinen Agent-Workflow verloren geht.

**Widerspruch als Qualitätsmerkmal:** Der Agent muss mit eigener Meinung gezeichnet sein: "sei ein erfahrener Kollege – sag auch mal, dass der Weg falsch ist, und gib zu, wenn du unsicher bist." Das unterscheidet einen Partner von einer Chatbot-Integration.

---

## 4b. Historische Einordnung: Rückkehr zum interaktiven Computing

Die Interaktionsform dieses Konzepts ist keine Erfindung, sondern eine Rückkehr. Das klassische Modell des Enterprise-Computing war schon immer "Pair Programming": Der Computer unterstützt und assistiert, macht aber **nie ungefragt Dinge** – er wartet auf Eingaben, reagiert auf Befehle, vollstreckt nichts Eigenständiges. Jahrzehnte lang war das keine Produktentscheidung, sondern das Selbstverständnis des Werkzeugs Computer.

Die Informatikgeschichte kennt genau diese Wende schon einmal: **Batch-Processing → interaktives Computing.** Im Batch-Zeitalter gab man ein Lochkartenpaket ab, die Maschine arbeitete allein, und man bekam später das Ergebnis zum Durchsehen – der Ur-Ablehn-Diff. Die Zeitteilungs-Systeme der 1960er und Lickliders "Man-Computer Symbiosis" beendeten das: plötzlich Dialog im Sekundentakt, und die produktivste Ära der Softwaregeschichte folgte. Heutige Agent-Tools sind strukturell **Batch-Processing mit aufgemotztem I/O**: ein "Auftrag abgeben", autonomer Lauf, hinterher Diff-Abnahme. Der Agent-Hype ist historisch gesehen ein Rückschritt hinter 1965.

Das Konzept ist damit auch als These lesbar: *Agenten + das klassische Interaktionsmodell = das fehlende Synthese-Produkt.* Nicht die Autonomie war die Innovation der Agenten, sondern die Modell-Fähigkeit – die Interaktionsform darf dabei trotzdem die des klassischen Assistenz-Computers bleiben.

---

## 5. Interaktionsmodell: Autonomie-Phasen

Der Satz *"ok, mach den Rest alleine"* braucht drei saubere Übergänge:

**Handoff (Übergang).**  
Der Agent kann nur das autonom weiterarbeiten, **was gemeinsam erarbeitet wurde**. Der Kontext der Session – diskutierte Ideen, Entscheidungen, offene Fragen – ist das gemeinsame Verständnis, das er mitnimmt. Er läuft nicht los, *weil er kann*, sondern weil man sich über den Rest geeinigt hat. Autonomie ist bei diesem Konzept das Ergebnis eines Dialogs, nicht der Startzustand.

**Solo (der Agent arbeitet allein).**  
Sichtbar, aber nicht zudringlich: eine Zusammenfassung, was er gerade tut – wie ein Kollege im Nebenzimmer. Keine live rotierende Diff-Ansicht, die zum passiven Zuschauen verleitet.

**Re-Sync (der Rückweg – der kritischste Teil für das Ausgangsproblem).**  
Der Agent ist fertig – jetzt kommt *nicht* "hier ist der Diff, viel Spaß beim Abnicken", sondern ein **Gespräch über das Ergebnis**:

- "Ich habe X anders gelöst als besprochen, weil Y – schau dir besonders Datei Z an."
- Die Änderungen werden als Story erzählt, mit explizit markierten Stellen, an denen seine Entscheidungen von der Abstimmung abweichen.
- Der Mensch liest das, was der Agent *entschieden* hat – nicht jede Zeile.

Re-Sync löst das Dilemma "90 % Agent-Code, aber ich will nicht alles lesen": Gelesen wird das, was Erklärung braucht, und die Erklärung ist ein Dialog, kein Diff.

---

## 6. Evidenz-Basis: Konzept-Elemente und ihre wissenschaftliche Fundierung

Die zentralen Design-Entscheidungen dieses Konzepts sind nicht intuitiv, sondern durch empirische Forschung gestützt. Übersicht der Zuordnung:


| Konzept-Element                                                           | Forschungsbefund                                                                                                                                                                                   | Quelle                                                                                                        |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Re-Sync als "Gespräch über Abweichungen" + Erklär-Modus (Teach-back)      | Ein erzwungenes "Explanation Gate" vor Übernahme von KI-Code halbierte die Fehlerquote in einer Wartungsaufgabe ohne KI (77 % → 39 %) bei gleicher Produktivität (N=78, Cursor-Plugin "VibeCheck") | [arXiv 2602.20206](search-result://I6RHSeJc)                                                                  |
| Handoff-Regel "Offloading ja, Outsourcing nein"                           | Theoretische Unterscheidung: extraneous Load abgeben (Offloading) erhält Lernen; intrinsische kognitive Last abgeben (Outsourcing) erzeugt "Epistemic Debt" und "fragile experts"                  | [arXiv 2602.20206](search-result://I6RHSeJc)                                                                  |
| Was-wäre-wenn-Annotationen als *Fragen* statt Feststellungen              | Gelenkte (gescaffoldete) Selbst-Erklärungen schlagen freie; der Self-Explanation-Effekt ist einer der robustesten Lernbefunde, speziell für Code-Comprehension                                     | [Self-Explanation Tutor, arXiv](search-result://TfITKRtb), [Springer/DeepCodeTutor](search-result://cztWKIbp) |
| Was-wäre-wenn als Vorher-Schau statt Nachher-Review                       | Generation-Effekt: Antworten produzieren, bevor man sie sieht, baut dauerhaftes Wissen auf; KI auf Abruf löscht genau Retrieval, Generation und Interleaving aus                                   | [Future Proof / Bjork](search-result://wkxBZ1yO), [AIEd-KB](search-result://tqkbBgNe)                         |
| Lautstärkeregler als *kontingenter* Mechanismus (in Modus adaptiv)        | Kontingentes Scaffolding (Support steigt bei ausbleibender Eigeninitiative, fällt bei Take-up) erreichte die Nachfrage eines zurückhaltenden Tutors bei doppelter Ausbeute (N=131)                 | [arXiv 2609.22993 / CoMeT](search-result://2yvtWgeh)                                                          |
| "Verständnis pro Stunde" als Zielgröße; Misstrauen gegen flüssiges Gefühl | Metakognitive Illusionen: Lernende unter KI überschätzen systematisch ihre Kompetenz; Leistung während des Lernens ist ein trügerischer Indikator                                                  | [The Widening Gap, arXiv](search-result://CXaIcehc), [Frontiers RCT N=122](search-result://M1L4x48F)          |
| Trigger "Save/Region verlassen" statt Unterbrechung mitten im Flow        | Feldstudie: Interventionen an Workflow-Grenzen 52 % Akzeptanz, Unterbrechungen mitten in der Aufgabe 62 % Ablehnung; gut getimte Vorschläge halbieren Interpretationszeit                          | [Codex Knowledge Base](search-result://lnYeSpjT)                                                              |
| Visuelle Präsenz + an Code-Stellen verankerte Gespräche (Breakouts)       | Studienevidenz aus dem Codellaborator-Prototyp: proaktiver Agent mit Cursor-Präsenz und lokal verankerten Threads verbesserte Kollaboration signifikant (N=18)                                     | [arXiv 2502.18658](search-result://zF5ojnom)                                                                  |
| Hypothese-bilden vor dem Agent-Lauf (Arbeits-Workflow)                    | Desirable Difficulties: Retrieval vor Lösungseinsicht,.spacing/interleaving statt geblockter Abrufhilfe                                                                                            | [Glasp](search-result://icNdFHQZ), [Taalhammer](search-result://zBdKvJe3)                                     |


**Zentrale theoretische Begriffe aus der Forschung** (für die spätere Doku/Prompts nutzbar):

- **Metakognitive Reibung** – das, was der Lautstärkeregler dosiert; Zukunftsvision der VibeCheck-Autoren: "Lernsysteme müssen metakognitive Reibung erzwingen" ([arXiv](search-result://I6RHSeJc)).
- **Offloading vs. Outsourcing** – die Regel für den Solo-Modus: Routine abgeben ist legitim, Denkarbeit verdecken nicht ([arXiv](search-result://I6RHSeJc)).
- **Nachfrage vs. Last** – ein System kann kognitive Last abnehmen (Hilfe) und gleichzeitig metakognitive Nachfrage hochhalten (Verstehen einfordern); das ist die theoretische Widerlegung des "du willst nur bremsen"-Einwands ([arXiv 2609.22993](search-result://2yvtWgeh)).
- **Desirable Difficulty** – Schwierigkeit muss im kognitiven Prozess liegen (Retrieval, Produktion, Diskriminieren), nicht im Reiz; Beliebiges Nerven hat nachweislich keinen Effekt ([Taalhammer](search-result://zBdKvJe3)).

**Vorbehalte:** Die VibeCheck-Studie rekrutierte AI-native Laien, nicht Senior-Entwickler; CoMeT testete Lernaufgaben, nicht Feature-Work; Schul-RCTs übertragen nicht automatisch auf Profis. Die Bjork'sche Basis ist dagegen Jahrzehnte-robust und domänenübergreifend. Detailanalyse in der separaten Bestandsaufnahme "Wissenschaftliche Grundlagen".

**Quellen-Herkunft:** Die Belege stammen aus zwei Dokumenten: Lern-/Kognitionsforschung (VibeCheck, Bjork/Desirable Difficulties, CoMeT, Self-Explanation, Metakognitions-Studien) → Bestandsaufnahme "Wissenschaftliche Grundlagen"; Markt-/Interaktionsforschung (Codellaborator N=18, Timing-Studie 52 %/62 %) → Bestandsaufnahme "Markt &amp; Forschung". Beide URIs sind jeweils dort im Quellenverzeichnis verankert.

---

## 7. Was das Konzept nicht ist (Non-Goals)

- **Kein "Prompt → Output"-Verkürzer.** Das Tool optimiert nicht auf Output pro Stunde, sondern auf *Verständnis pro Stunde*. Es ist bewusst langsamer als Claude Code – das ist kein Bug, es ist der Zweck.
- **Kein unsichtbarer Agent.** Alles, was der Agent tut, ist sichtbar und begründet. Keine Hintergrund-Aktionen jenseits des geteilten Kontexts.
- **Kein Auto-Accept, nirgends.** Jede Übernahme von Agent-Code ist eine bewusste Handlung des Menschen – mit Ausnahme des vereinbarten Solo-Modus, der per Dialog-Handoff autorisiert wurde.
- **Kein Reibungs-Verschwinder.** Reibung bleibt als gestaltbares Feature erhalten (Lautstärkeregler, manuelle Übernahmen), nicht als Zufall.

---

## 8. Entscheidungen (ehemalige offene Fragen)

Die ursprünglich offenen Fragen sind wie folgt entschieden:

1. **Session vs. Daueranwesenheit → Session-basiert.** Das Tool wird gestartet; eine Pair-Session gehört zu einem Arbeitsvorhaben (wie ein Meeting mit einem Kollegen). Keine Daueranwesenheit im Hintergrund.
2. **Persistenz → Ja, als Markdown-Gedächtnis.** Für V1 genügt ein einfaches Markdown-Gedächtnis (Session-Protokoll + aufeinander aufbauende Notizen), das beim Session-Start geladen wird. Kein Vektor-Store, keine Datenbank – bewusst simpel und für den Menschen les-/editierbar.
3. **Team-Fähigkeit → nicht vorgesehen.** Persönliches Werkzeug, keine geteilten Sessions, keine Team-Features.
4. **Provider-Unabhängigkeit → mehrere Provider vorsehen.** Offen gelassen, ob "Partner-Qualität" pro Provider unterschiedlich definiert wird – sehen wir dann.
5. **Abgrenzung vom Bestehenden → recherchiert.** Die Bestandsaufnahme (separater Canvas) hat ergeben: Einzelelemente existieren, die Kombination aus Dialog, Deixis und abgestufter Autonomie nicht.
6. **Der Nerv-Faktor → durch Forschung beantwortet.** Die Timing-Studie liefert die empirische Grundlage: Trigger an Workflow-Grenzen (Save, Region verlassen) statt mitten im Flow; dazu der kontingente Regler (CoMeT-Mechanik) als Dosierung. Die Recherche-Details stehen im Evidenz-Abschnitt.
7. **Host → eigene, dünne VS Code-Extension mit hostunabhängigem Core.** Selection, Save-Events und Regler-UI sind Editor-nativ; die Interaktionsform ist das Produkt und verlangt den eigenen Rahmen. Der Core (Session, Markdown-Gedächtnis, Provider-Layer, Gate) lebt hostunabhängig hinter einem schmalen Host-Adapter – unit-testbar ohne Editor, VS Code-Forks teilen denselben Adapter. Stack: TypeScript, Webview mit React + Tailwind (CSP-bewusst gebaut, Theme-Token auf die Editor-Variablen). Design ist erklärtes Qualitätsziel – das Tool soll gut aussehen und Spaß machen; V0.1 poliert die eigenen Flächen (Frage-Karte, Regler, Chat), keine komplette visuelle Identität. Terminal-first (z.B. OpenCode) schied aus: Es würde §3.2/§9.2 aushebeln und genau den Teil nie testen, der das Tool von einem Chat-Plugin unterscheidet.

---

## 9. MVP: Version 0.1 (Dogfood-fähig)

Ziel: Das Tool so schnell wie möglich produktiv nutzbar machen – und dann **das Tool zur Entwicklung dieses Tools einsetzen** (Dogfooding). Jede Session am eigenen Quelltext ist zugleich Entwicklung und Echt-Test.

**Leitlinie fürs MVP:** Die Dialog-Tragfläche minimal, Deixis als Light-Version, Autonomie-Phasen ganz außen vor.

**V0.1 enthält:**

1. **Session-Start/-Stopp** – Kommando startet eine Pair-Session (entscheidung 1: session-basiert).
2. **Chat-Sidebar mit geteiltem Kontext** – Agent sieht aktive Datei, Selection und Terminal-Output des Menschen; Chat ist der Dialog-Kanal.
3. **Save-Trigger** – Beim Speichern/Verlassen einer Region darf der Agent einen kurzen Kommentar abgeben (2–3 Sätze), erscheint im Chat mit file:line-Verweis. Form nach Inhalt: Feststellung für objektive Bugs; für Design-Zweifel eine **Frage mit aufklappbarer Antwort** – die Antwort ist erst nach eigener Überlegung sichtbar (Generation-Fenster, gelenkte Selbst-Erklärung). Reveal-Klick, ausbleibende Reaktion und Moduswechsel werden ab Tag 1 als Verhaltenssignale ins Session-Protokoll geloggt – vom Gate in V0.1 noch ignoriert, aber Datenbasis für die spätere Kontingenz (Dogfood = Evidenzfenster). Das ist Deixis-Light: Zeigen per Link statt per Highlight.
4. **Lautstärkeregler, zwei Stufen** – *Still* (nur auf explizite Fragen) und *Kollege* (Save-Trigger aktiv). Die dritte Stufe und die CoMeT-Kontingenz kommen später.
5. **Multi-Provider** – provider-neutraler API-Layer (direkte APIs und/oder OpenRouter), Modell konfigurierbar (Entscheidung 4).
6. **Markdown-Gedächtnis** – Session-Protokoll + wachsende Notiz-Datei, beim Start geladen, für den Menschen les- und editierbar (Entscheidung 2).

**V0.1 enthält bewusst NICHT:** Solo-Modus + Re-Sync (Autonomie-Phasen), Inline-Highlights/Breakouts/Gutter-Marker (echte Deixis), Was-wäre-wenn-Annotationen, kontingente Regler-Mechanik, Erklär-Modus/Teach-back.

**Begründung der Reihenfolge:** Dialog zuerst – er ist die Tragfläche, ohne die das Tool von jedem Chat-Plugin nicht mehr unterscheidbar wäre, und er liefert sofort täglichen Nutzen (die Save-Trigger-Kommentare sind der "Kollege am Nebentisch"). Deixis-Light per file:line-Verweis kostet fast nichts und testet, ob der Zeig-Gest-Ansatz im Alltag trägt, bevor Inline-Deko gebaut wird. Autonomie-Phasen sind am aufwendigsten und brauchen den Dialog als Fundament – sie kommen, wenn das tägliche Dogfooding zeigt, dass der Boden steht.

**Dogfood-Zyklus:** Nach jedem Arbeitstag am Tool eine kurze Selbstreflexion im Markdown-Gedächtnis: Was hat der Agent kommentiert, was davon war gut, was hat genervt? Das ist zugleich die Empfehlung 3 aus der Forschungs-Bestandsaufnahme (das Tool erzeugt selbst seine Evidenz).

---

## 10. Zusammenfassung in einem Satz

Ein Agent-Tool, das den Menschen zum verstehenden Co-Autor macht statt zum müden Reviewer: Dialog im Minuten-Takt, gemeinsames Zeigen im Editor, und Autonomie nur als verabredeter Modus mit sauberem Rückweg.
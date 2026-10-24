# Pair Agent – Bestandsaufnahme: Markt, Forschung, Workarounds

*Stand: 23. Oktober 2026. Bewertung: Wie nah kommen bestehende Werkzeuge, Harnesses und Forschungsprototypen an die drei Tragflächen des Konzepts (Dialog, Deixis, abgestufte Autonomie)? Wo bleibt die Lücke?*

---

## Fragestellung

Gibt es bereits Werkzeuge oder Forschung, die ein echtes Pair Programming mit einem Agenten ermöglichen – enger Dialog im Minuten-Takt, beidseitiges Zeigen im Editor, und Autonomie nur als verabredeter Modus mit sauberem Rückweg? Und: Wie lösen andere Entwickler und Anbieter das Problem der Verflachung und des passiven Konsumierens?

---

## Executive Summary

1. **Die Lücke ist real, aber kleiner als gedacht.** Einzelfeilemente existieren überall – Plan-Modi, Lern-Modi, proactive Agenten, annotierte Kommentare – aber **niemand kombiniert die drei Tragflächen zu einem Produkt**. Der nächste Verwandte ist ein Forschungsprototyp, kein Werkzeug.
2. **Der nächstliegende Verwandte ist ein Forschungsprototyp namens Codellaborator** (2025): ein proaktiver Agent mit eigener Cursor-Präsenz im Editor und an Code-Stellen verankerten Gesprächen ("Breakouts") – fast exakt unsere Deixis-Tragfläche. Er ist aber nicht verfügbar, hat keine Autonomie-Phasen und wurde nie produktiviert.
3. **Aider nennt sich selbst "AI pair programming", ist aber rein reaktiv und turn-based** – kein ungefragtes Kommentieren, kein Zeigen, keine Autonomie-Übergänge.
4. **Claude Code hat mit "Learning Mode" die bisher nächste kommerzielle Annäherung** an Verständnis-Erhalt: TODO (human)-Marker, bei denen der Mensch selbst Code schreibt. Es ist aber ein Einzeltraining, kein Dialog unter Partnern.
5. **Das Ausgangsproblem ist messbar und gesellschaftlich etabliert:** Anthropics eigene RCT zeigt \~17 % schlechtere Code-Comprehension bei KI-gestützten Entwicklern; die Debatte heißt "skill atrophy" und ist 2026 ein Mainstream-Thema.
6. **Die Industrie bewegt sich in die entgegengesetzte Richtung:** Cursors eigene Daten zeigen 2,5× größere PRs und 5-faches Wachstum von Agent-Commits ohne manuelle Diff-Abnahme. Die\_review\_-Seite wird auf AI-Code-Reviewer (Bugbot, Auto-review) ausgelagert – Verständnis beim Menschen ist nirgends ein Produktziel.
7. **Der Workaround-Markt existiert und wächst:** Review-Layer (Plannotator, AQ), disziplin-encoding Skills (Osmani),Approval-Workflows. Alles Worksheet-Ökonomie – es erzwingt Prüfung, aber kein *gemeinsames Verstehen*.

---

## Methodik

Zwei Recherche-Runden mit Breitensuche (Harnesses, Forschungsprototypen, Skill-Atrophy-Debatte, Review-Workarounds) plus News-Suche zu Marktveränderungen 2026. Quellen: offizielle Doku, arXiv-Paper, Hersteller-Blogs, Entwickler-Analysen. Limitation: AI-Coding-Tools ändern sich monatlich; Angaben zu aktuellen Versionen sind Momentaufnahmen (Stand Oktober 2026). Klickzahlen und Nutzerzahlen stammen aus Sekundärquellen.

---

## Befunde

### 1. Etablierte Harnesses: Was sie heute können – und was nicht

**Claude Code (Anthropic)** hat die differenzierteste Modus-Landschaft: Plan Mode als read-only Zustand mit expliziter Plan-Abnahme ([Claude Community](search-result://Zr6Hz80j), [The Prompt Shelf](search-result://3Bg17eAU)); die vier Output-Styles Default/Proactive/Explanatory/**Learning** ([The Prompt Shelf](search-result://x8VkQjNx)); und Learning Mode als pair-artigste kommerzielle Funktion: Claude setzt TODO (human)-Marker und lässt den Menschen 5–10 strategisch wichtige Zeilen selbst schreiben ([Learning-Style-Plugin auf GitHub](search-result://emLs3zhl), [Tessl](search-result://jWsKwC7M)). Was fehlt: Learning Mode ist ein Lehrlings-Modus ("Claude gibt dir Aufgaben"), kein Partner-Dialog; es gibt kein beidseitiges Zeigen; die Agenten-Loop bleibt Auftrags-logisch ([Claude Code Docs](search-result://w1krSNqv)).

**Cursor** ist der klare Gegenpol: agentische Autonomie ist das Produkt. Hintergrund-Agents arbeiten asynchron in isolierten Worktrees und öffnen eigenständig PRs; eigene Daten zeigen p75-Zeilen pro PR +2,5× und 5-faches Wachstum von Commits ohne manuellen Diff-Akzeptanzschritt ([Cursor Guide](search-result://Quj5waEL)). Die Antwort auf das Review-Problem ist ein **AI-Reviewer** (Agent Review, Bugbot, Auto-Review), nicht menschliches Verständnis ([Cursor Research](search-result://aMaPDmn5)). Cursor selbst beschreibt die neue Rollenverteilung: Agent plant → Mensch reviewt → Agent implementiert ([Arize](search-result://jr8rGJR8)) – der Mensch bleibt Reviewer, wird nie Partner.

**Codex CLI (OpenAI)** hat einen interaktiven TUI-Modus, den eine Community-Analyse explizit als "Pair Programming" rahmt – mit dem ehrlichen Hinweis auf die fundamentale Asymmetrie: der Mensch ist immer Navigator, der Agent immer Driver; ein Rollenwechsel wie beim menschlichen Pairing existiert nicht ([Codex Knowledge Base](search-result://iOerNid6)). Seit April 2026 gibt es "ambient suggestions" – proaktive Vorschläge beim Session-Start ([Codex Knowledge Base](search-result://k8Ly2FxZ)); das ist Proaktivität für *Aufgaben*, nicht für *Dialog über Code*. Bemerkenswert für uns: **Codex ist als offener Harness dokumentiert** ("Codex as a platform"), was für ein eigenes Tool als Fundament interessant wäre ([OpenAI Developers](search-result://ucdRJVkH)).

**Aider** ist das produktivste "Pair Programming"-Label am Markt: CLI, archiviert alles in Git, beobachtet Dateien und reagiert auf Kommentare im Editor, chat modes (code/architect/ask) ([aider.chat](search-result://lDBMQbDb), [GitHub](search-result://4QpWgWX7)). Architekt-Modus (Planner schreibt Spec, Editor implementiert) ist eine Rollentrennung – aber zwischen zwei Agenten, nicht zwischen Mensch und Agent ([Digital Applied](search-result://8Ijw2Egw)). Kein proaktives Kommentieren, keine Inline-Markierungen, kein Zeigen.

**Fazit Harnesses:** Alle vier bieten *Modi der Deceleration* (Plan/Learning/Approvals) an, aber Interaktion bleibt auftragszentriert: Der Mensch erteilt oder prüft. **Kein einziges Werkzeug hat einen ungefragten, kontextbezogenen Kommentar-Kanal im Editor.**

### 2. Forschung: Die Ideen existieren – in Papierform

**Codellaborator** (Cornell/UIUC, Feb 2025) ist der wichtigste Fund: ein proaktiver Agent im Editor mit eigener visueller Präsenz (Cursor + Caret, die Aufmerksamkeitsfokus signalisieren) und "Breakouts" – lokal begrenzte Gesprächs-Threads, die an konkreten Code-Stellen verankert sind. In einer 18-Personen-Studie verbesserte die proaktive Variante die Kollaboration signifikant ([arXiv 2502.18658](search-result://zF5ojnom)). Das ist fast wörtlich unsere Deixis-Tragfläche – als unveröffentlichter Studienprototyp ohne Produktwerdung, ohne Autonomie-Phasen, ohne Lautstärke-Regler.

**Proaktivität statt Autonomie** wird als Forschungsthema 2026 explizit formuliert: Bui &amp; Evangelopoulos argumentieren, dass die Industrie die falsche Achse optimiert – nicht mehr Autonomie, sondern bessere Entscheidung darüber, *wann* man sich meldet, mache den Unterschied zwischen "nützlichem Kollegen und teurer Zwischenablage" ([Codex Knowledge Base](search-result://lnYeSpjT)). Eine Feldstudie mit 15 Entwicklern zeigt das Timing-Problem quantifiziert: Interventionen an Workflow-Grenzen erreichten 52 % Akzeptanz, Unterbrechungen mitten in der Aufgabe 62 % Ablehnung ([Codex Knowledge Base](search-result://lnYeSpjT)) – direkt relevant für unseren Lautstärkeregler und die Trigger-Frage.

**Weitere Papiere:** CMU/Microsoft haben Design-Kriterien für proaktive Assistenz entwickelt (Timing, unaufdringliche Präsentation) ([arXiv 2410.04596](search-result://bHRKmWFi)); ProPACT modelliert Pair Programming als gemeinsame Aufmerksamkeits-Regulation mit Eingriffen bis zu 30 Sekunden *vor* dem Zusammenbruch der Kollaboration ([arXiv 2605.02703](search-result://zA2yz8X8)); und eine Doktorarbeit untersucht Pair-Workflows, in denen LLM-Verhalten gegen Spezifikationen validiert wird ([arXiv 2604.10300](search-result://QRr9iLK4)).

### 3. Das Ausgangsproblem: Messbar und Mainstream

Die Verflachung ist kein subjektives Einzelschicksal: Anthropics eigene RCT zeigte \~17 % niedrigere Comprehension-Scores bei Entwicklern, die mit Claude Code lernten vs. ohne ([Senwitt](search-result://s9u1rb5v), [AgentPatterns](search-result://6fzD13I6)). Karpathy beschrieb den Übergang zu "80 % Agent-Coding, ich programmiere hauptsächlich auf Englisch" ([Osmani](search-result://htjubb79)); Armin Ronachers Umfrage unter 5.000 Entwicklern: 44 % schreiben selbst weniger als 10 % ihres Codes ([Osmani](search-result://htjubb79)). Die Analyse-Community formuliert Gegenmaßnahmen, die unserem Workflow ähneln: "Ask conceptual questions / code independently / explain-then-code" erhalten Lernen, "accept without understanding" zerstört es ([AgentPatterns](search-result://6fzD13I6)). Der AP berichtet über den Arbeitsmarkt, dass Firmen jetzt gezielt Leute suchen, "die AI-Tools beaufsichtigen können, ohne ihre Ausgaben blind zu akzeptieren" ([AP](search-result://kxYmDa8Y)). **Unser Konzept adressiert ein dokumentiertes, benanntes, wachsendes Problem.**

### 4. Workarounds: Der illegitime Vorfahre unseres Tools

Um die Review-Blindheit zu mildern, ist eine kleine Werkzeugklasse entstanden:

- **Plannotator** – lokale Review-Schicht für Agent-Pläne und Diffs; Annotationen fließen als nächster Prompt zurück an den Agenten ([VibecodingHub](search-result://Ujosh3XC)).
- **AQ** – Review über die Session statt über den Diff: Man sieht das Verhalten der laufenden App, Kommentare in der Preview werden zur Agent-Instruktion ([aq.dev](search-result://grFiEnao)).
- **Anchored Comments (VS Code Extension)** – an Code verankerte Kommentar-Threads mit file:line-Deeplinks, die explizit Claude-Code- und Codex-Konversationen verlinken – ein Deixis-Baustein, aber statisch und mit 8 Installationen ([VS Marketplace](search-result://B4krTFY3)).
- **Addy Osmanis agent-skills** – 23 Workflows, die Agenten Senior-Disziplin aufzwingen (Spec vor Code, anti-rationalization tables) ([GitHub](search-result://mo9r0A4O), [Osmani](search-result://mYWgEI7s)).

**Gemeinsame Grenze:** Alle erzwingen *Prüfung*, keins erzeugt *gemeinsames Verstehen*. Der Dialog-Gedanke und die proaktive Anwesenheit fehlen überall.

---

## Lücken-Analyse: Position unseres Konzepts


| Tragfläche               | Nächster Verwandter                                        | Was fehlt dort                                                                                     |
| ------------------------ | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Dialog**               | Claude Code Learning Mode                                  | Lehrlings-Modus, keine Partner-Rollen, keine ungefragten Kommentare                                |
| **Deixis**               | Codellaborator (Forschung), Anchored Comments (8 Installs) | Forschungsprototyp bzw. statische Threads; kein lebendiges Zeigen, keine Was-wäre-wenn-Vorherschau |
| **Abgestufte Autonomie** | Cursor Handoff + PR-Review, Claude Code Plan Mode          | Autonomie als Default statt Dialog-Ergebnis; Re-Sync = Diff, nie "Gespräch über Abweichungen"      |
| **Lautstärke/Timing**    | Proaktivitäts-Forschung (Timing-Daten vorhanden)           | Kein Produkt hat einen Kolleglichkeits-Regler                                                      |


**Kernaussage:** Nichts an diesem Tisch ist unbedeckt – aber **niemand sitzt an allen vier Plätzen gleichzeitig.** Die Kombination ist die Innovation, nicht das Einzelelement. Geplanter Umgang mit dem Nachbau-Risiko: Das Projekt wird open source. Wenn ein großer Anbieter die Idee übernimmt und besser umsetzt, ist das ein Erfolg, kein Verlust – das Ziel ist die Interaktionsform als Branchenstandard, nicht Marktanteil. Die Forschung liegt öffentlich; ein solcher Nachbau validiert das Konzept und verschafft dem Werkzeug über Mitwirkende und Forks weiter Wirkung. Die eigentliche Erfolgsmetrik ist daher nicht Konkurrenzabwehr, sondern: setzt sich "Verständnis pro Stunde" als legitimes Produktziel durch?

---

## Quellenverzeichnis


| Quelle                                                                            | Glaubwürdigkeit | Stand    |
| --------------------------------------------------------------------------------- | --------------- | -------- |
| [arXiv 2502.18658 – Codellaborator](search-result://zF5ojnom)                     | 5/5             | Feb 2025 |
| [arXiv 2410.04596 – Need Help? (CMU/Microsoft)](search-result://bHRKmWFi)         | 5/5             | Okt 2024 |
| [arXiv 2605.02703 – ProPACT](search-result://zA2yz8X8)                            | 4/5             | Mai 2026 |
| [arXiv 2604.10300 – From Helpful to Trustworthy](search-result://QRr9iLK4)        | 4/5             | Apr 2026 |
| [Codex Knowledge Base – Proactivity, Not Just Autonomy](search-result://lnYeSpjT) | 3/5             | Jul 2026 |
| [Codex Knowledge Base – Codex CLI Pair Programming](search-result://iOerNid6)     | 3/5             | Apr 2026 |
| [OpenAI Developers – Codex as a platform](search-result://ucdRJVkH)               | 4/5             | 2026     |
| [aider.chat – Dokumentation](search-result://lDBMQbDb)                            | 4/5             | 2026     |
| [GitHub – Aider-AI/aider](search-result://4QpWgWX7)                               | 4/5             | 2026     |
| [The Prompt Shelf – Claude Code Output Styles](search-result://x8VkQjNx)          | 3/5             | Aug 2026 |
| [GitHub – anthropics/claude-code Learning Plugin](search-result://emLs3zhl)       | 5/5             | 2025     |
| [Tessl – Claude Code output styles](search-result://jWsKwC7M)                     | 3/5             | Sep 2025 |
| [Claude Code Docs – How Claude Code works](search-result://w1krSNqv)              | 5/5             | 2026     |
| [Claude Community – Plan Mode](search-result://Zr6Hz80j)                          | 3/5             | Sep 2026 |
| [The Prompt Shelf – Plan Mode](search-result://3Bg17eAU)                          | 3/5             | Jun 2026 |
| [Cursor – AI code review Guide](search-result://Quj5waEL)                         | 4/5             | 2026     |
| [Cursor – Governing agent autonomy](search-result://aMaPDmn5)                     | 4/5             | Jun 2026 |
| [Arize – Inside Cursor's Agent Factory](search-result://jr8rGJR8)                 | 3/5             | Jul 2026 |
| [AddyOsmani – The 80% Problem](search-result://htjubb79)                          | 4/5             | Jan 2026 |
| [AddyOsmani – Agent Skills](search-result://mYWgEI7s)                             | 4/5             | Mai 2026 |
| [GitHub – addyosmani/agent-skills](search-result://mo9r0A4O)                      | 4/5             | Jun 2026 |
| [AgentPatterns – Skill Atrophy](search-result://6fzD13I6)                         | 3/5             | 2026     |
| [Senwitt – Anthropic RCT Analyse](search-result://s9u1rb5v)                       | 3/5             | Mai 2026 |
| [Joshua Davis – The Atrophy Risk](search-result://DPMYP7jV)                       | 3/5             | 2026     |
| [AP – Coding-Boom-Auswirkungen](search-result://kxYmDa8Y)                         | 5/5             | Sep 2026 |
| [VibecodingHub – Plannotator](search-result://Ujosh3XC)                           | 3/5             | Jul 2026 |
| [aq.dev – Session-Review](search-result://grFiEnao)                               | 3/5             | Jul 2026 |
| [VS Marketplace – Anchored Comments](search-result://B4krTFY3)                    | 3/5             | 2026     |
| [Safeguard – Approval Workflows](search-result://BgDrrs4K)                        | 3/5             | Okt 2026 |
| [Ones.com – Reviewable Agent Actions](search-result://RxjVTvgo)                   | 2/5             | 2026     |


**Konflikte &amp; Vorbehalte:** Die Teilnehmerzahl der Anthropic-RCT wird uneinheitlich angegeben (52 vs. 400 Teilnehmer) – die Kernaussage (\~17 % Comprehension-Rückstand) stimmen mehrere Quellen überein, die genaue Zahl sollte vor Zitierung im Originalland geprüft werden. Sekundärquellen mit eigenem Produktinteresse (aq.dev, Safeguard, Ones) sind als Darsteller ihres eigenen Werkzeugs zu lesen.

---

## Offene Fragen

1. **Adoptierbarkeit von Forschung:** Codellaborator ist Closed-Source-Forschung – wie viel davon ist mit heutigen Editor-APIs (VS Code decorations, inline chat) in Eigenregie nachbaubar?
2. **Provider-Risiko:** Wie schnell kann Claude Code Learning Mode zu einem echten Dialog-Modus ausgebaut werden, wenn Anthropic es wollte? (Ihre eigene RCT zeigt, dass sie das Problem kennen.)
3. **Timing-Grenzwerte:** Die 52 %/62 %-Zahlen stammen aus einer kleinen Feldstudie – wie kalibriert man den Lautstärkeregler robust für verschiedene Entwickler-Typen?
4. **Business-Frage:** Wer zahlt für "Verständnis pro Stunde"? Einzelpersonen (Karriere- Angst) oder Teams (Wartbarkeit, Junior-Pipeline)?

---

## Empfehlungen / Nächste Schritte

1. **Codellaborator-Paper vollständig lesen** – es ist der wichtigste Beleg, dass die Deixis-Tragfläche funktioniert, und enthält vermutlich die meisten Antworten auf UI-Fragen, die wir uns noch stellen.
2. **Timing-Studien (Proactivität) in das Konzept integrieren** – die Trigger-Regeln im Konzept-Doc um empirische Akzeptanz-Zahlen ergänzen; das schützt vor dem Clippy-Fehlschlag.
3. **"Codex as a platform" als möglichen Unterbau evaluieren** – ein offener Harness mit eigener UI darauf zu setzen ist realistischer als ein Komplettbau; das ist aber eine Implementierungsfrage, die erst nach dem Konzept-Freeze entschieden werden muss.
4. **Positionierung als Open-Source-Projekt definieren:** Leitmotiv "Verständnis pro Stunde statt Output pro Stunde". Wenn ein großer Anbieter es übernimmt und besser macht: umso besser – Ziel ist, dass die Interaktionsform zum Standard wird, nicht Marktanteil. Lizenz, Governance und Beitragungsmodell klären, bevor die erste Zeile öffentlich ist (z.B. Contributor-Freundlichkeit für genau die Entwickler, die das Verflachungsproblem selbst haben).
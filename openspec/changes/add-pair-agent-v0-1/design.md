# Design

## Context

Greenfield: Das Repo enthält außer den Konzeptdokumenten (SPEC.md, RESEARCH.md) keinen Code. Motivation siehe proposal.md ("Why"). Die tragenden Entscheidungen sind in SPEC.md §8 (1–7) festgehalten; dieses Design übersetzt sie in eine Architektur. Projekt-Kontext aus `openspec/config.yaml`: keiner (nur Schema `spec-driven`) – die Konzeptdokumente sind die verbindliche Quelle.

## Goals / Non-Goals

**Goals:**

- Die Interaktionsform ist das Produkt: Dialog-Takt, geteilter Kontext, dosierte Kommentare – architektonisch nicht nachträglich einbaubar, sondern von Anfang an das Rückgrat.
- Der Core (Session, Gedächtnis, Provider, Gate, Trigger) ist **unit-testbar ohne Editor**.
- Der Lautstärkeregler ist **mechanisch ehrlich**: Code garantiert die Erlaubnis-Grenze, egal was das Modell tut.
- Dogfood ab Tag 1: lokales vsix, tägliche Nutzung, Signal-Log füllt sich von Anfang an.
- Designqualität auf den drei eigenen Flächen (Frage-Karte, Regler, Chat); Theme-fähig (hell/dunkel/kontrastreich).

**Non-Goals:**

- Autonomie-Phasen (Handoff/Solo/Re-Sync), echte Deixis (Highlights, Gutter-Marker, Breakouts), Was-wäre-wenn-Annotationen, kontingente Regler-Mechanik, Teach-back-Gate (SPEC.md §9 "bewusst NICHT").
- Team-Features, Daueranwesenheit, Auto-Accept in jeder Form, Marketplace-Distribution.
- Eine fertige visuelle Identität (siehe D7: drei Flächen polieren, kein Brand-System).

## Decisions

### D1 – Zwei-Teil-Architektur: dünne Extension + hostunabhängiger Core

```
+------------------------------------------------------+
|        VS Code Extension (dünn, austauschbar)        |
|    Webview-Chat · Regler · Save-Events · Selection   |
|              kleiner Host-Adapter                    |
+-------------------------+----------------------------+
                          |  HostPort-Interface:
                          |  onSave, getActiveFile, getSelection,
                          |  getRecentTerminalOutput, zeigeChatNachricht,
                          |  getReglerStufe
                          v
+------------------------------------------------------+
|        Core (hostunabhängig, unit-testbar)           |
|   Session · Markdown-Gedächtnis · Provider-Layer     |
|   Proaktivitäts-Gate · Trigger-Engine · Signal-Log   |
+------------------------------------------------------+
```

Der Adapter ist derselbe Seam-Gedanke wie beim Gate: Die Trigger-Engine braucht Editor-Events ohnehin; ein schmales Interface kostet fast nichts und macht den Core ohne VS Code testbar. Repo-Layout: `src/core/`, `src/extension/`, `src/webview/` in einem TypeScript-Paket (Vite für den Webview-Teil, esbuild/tsc für Extension + Core, Vitest für Core-Tests).

*Alternativen:* Plugin auf bestehendem KI-Host (VibeCheck-Präzedenz, N=78) – verworfen: Ein geliehener Rahmen unterläuft den eigenen Test (die Interaktionsform ist die These), und Host-Lock kollidiert mit "persönliches Werkzeug, multi-provider". Terminal-first (z.B. OpenCode) – verworfen: Selection ist nur per Editor-API zu haben; es würde SPEC §3.2/§9.2 aushebeln.

### D2 – Erlaubnis vs. Dosierung als zwei Ebenen

Der Lautstärkeregler setzt die **Erlaubnis** (mechanisch, code-enforced): In Still gibt es *keinen* ungefragten Kommentar, punktum. Die **Dosierung** trifft ein Proaktivitäts-Gate an genau einer Stelle im Code: Es nimmt (Modus, Trigger, Budget) und wählt schweigen/sprechen. Invarianten im Code: Still → nie ungefragt; max. 1 Kommentar pro Save. *Alternative:* `if mode == kollege`-Checks verteilt im Code – verworfen, weil die CoMeT-Kontingenz (V0.2+) dann zum Umbau wird. Später: Kontingenz verändert nur Gate-Parameter (Cooldown, Leiter), keine neuen Codepfade.

### D3 – Code garantiert Grenzen, Modell urteilt im Budget

Der Code legt Invarianten und Raten-Limits fest. Das Modell bekommt pro Trigger: Code-Kontext, geteilten Kontext, ein Budget (max. 1 Kommentar) und die Form-Regeln im Prompt – und entscheidet, *ob* sich ein Kommentar lohnt ("Änderung trivial → schweigen") und *welche Form* sie hat (Feststellung bei objektivem Fehler, Frage-mit-Reveal bei Design-Zweifel). *Alternativen:* Volle Modellautonomie – verworfen, eine Grenze, die ein chattiges Modell per Prompt-Auslegung übersehen kann, ist keine Grenze. Volle Determinierung – verworfen, das Dosierungs-Urteil ist sozial-nuanciert und gehört ins Modell.

### D4 – Gestuftes Trigger-Vokabular; V0.1-Kollege = Save/Region-Trigger

Die Regler-Stufen unterscheiden sich im Trigger-Vokabular: Still = direkte Fragen; Bei Fragen = + Save/Region verlassen; Kollege = + echte Agenten-Initiative. V0.1 implementiert nur die ersten beiden Spalten (der V0.1-Kollege ist de facto "Bei Fragen"). Die dritte Spalte kommt später als neues Trigger-Vokabular *im selben Gate* – daher D2s zentrale Stelle.

### D5 – Reveal-Mechanik als Frage-Karte

Die Frage mit aufklappbarer Antwort ist eine eigene Karte im Chat: Frage sichtbar, Antwort hinter einem klaren Reveal-Element (Generation-Fenster: die Antwort ist erst nach eigener Überlegung sichtbar). Reveal-Klick deckt auf und loggt das Signal. Spätere Formen (Multiple Choice, Was-wäre-wenn) sind Form-Varianten derselben Karte. Begründung: mikro-Explanation-Gate ohne VibeChecks Zwang – dosierbar über den Regler.

### D6 – Signale: Event-Typen und Speicherort

Vier Event-Typen ins Session-Protokoll (Markdown, zeitstempelt): `mode_changed` (inkl. Richtung), `reveal_clicked`, `comment_ignored` (kein Reveal/keine Antwort innerhalb von 10 Minuten – Wert im Dogfood kalibrieren), `comment_posted` (Kontext für Interpretation). V0.1 liest die Signale nicht; Ausbaupfad: V0.2 Cooldown (N Saves Ruhe nach Interaktion, ein Gate-Parameter), V0.3+ CoMeT-Leiter (schnell runter, langsam rauf) mit kaltem Start aus dem Markdown-Gedächtnis.

### D7 – Webview-Stack und Design-Begrenzung

React + Vite + Tailwind im Webview. CSP-Konformität: kein Inline-Script, statisch gebundelte CSS-Datei (Tailwind-Build), keine Remote-Ressourcen. Theme-Token auf die Editor-Variablen (`--vscode-foreground` u.a.) plus `vscode-light`/`vscode-dark`-Body-Klassen – die Sidebar fügt sich ins Editor-Theme. Das offizielle Webview UI Toolkit ist deprecated und wird nicht verwendet. Designbudget: Frage-Karte, Regler, Chat – ruhig, themegerecht, typografisch sauber; keine visuelle Identität.

### D8 – Provider-Layer: Interface + Mistral als erster Adapter

Provider-neutraler Layer (`frageStellen(messages, kontext) → Antwort`); V0.1 liefert genau einen Adapter – **direkt gegen Mistral** (API-Zugang vorhanden). Das Interface hält weitere direkte Provider (Anthropic, OpenAI, …) und OpenRouter als spätere Drop-ins frei. API-Key in VS Code SecretStorage; Provider + Modell in den Extension-Settings. *Alternative:* OpenRouter als erster Adapter (ein Zugang, viele Modelle – ideal für den Modellvergleich) – verworfen für V0.1: Mittelsmann (Latenz, Extra-Account, teils Gebühren), obwohl die offene Frage aus Entscheidung 4 ("Partner-Qualität pro Provider") dafür spräche; der neutrale Layer erlaubt den Wechsel später ohne Umbau.

### D9 – Regler-Default und Gedächtnis-Speicherort

Jede Session startet auf **Still** – Autonomie (hier: Initiativrechte) ist nie der Default, der Mensch schaltet hoch (SPEC §2/§5-Geist). Das Markdown-Gedächtnis liegt projektgebunden unter `.pair/` im Workspace: `memory.md` (wachsende Notizen) + `sessions/<datum>.md` (Protokolle) – lesbar/editierbar, kein Vektor-Store (Entscheidung 2). *Alternative:* `~/.pair` – verworfen für V0.1, weil Notizen arbeitsvorhabenbezogen sind; Umzug bleibt einfach (Core bekommt nur einen Pfad).

### D10 – Terminal-Output über Shell Integration, mit Degrade

Der geteilte Kontext nutzt die VS Code Shell-Integration-API (letzter Befehl + Output). Ohne aktive Shell-Integration degradiert V0.1 bewusst: Kontext ohne Terminal-Output, ein sichtbarer Hinweis in der Sidebar – kein harter Fehler, keine stillegende Abhängigkeit.

## Risks / Trade-offs

- [Modell zu geschwätzig → Klippy-Risiko] → Raten-Limits im Code (max. 1/Save), Session-Default Still, Signal-Log macht die Dosierung im Dogfood sichtbar.
- [Frustration durch Reveal-Fragen] → Frage-Form nur bei Design-Zweifeln, Feststellung bei objektiven Bugs, Still-Modus als jederzeitiger Notausgang; Ausbaupfad Cooldown statt sofortiger Kontingenz.
- [Terminal-Output fehlt ohne Shell Integration] → bewusste Degrade mit Hinweis (D10), Dokumentation im README.
- [Provider-Variabilität der Partner-Qualität] → Form-Regeln und Persona im Prompt provider-unabhängig; Dogfood-Reflexion pro Modell (Entscheidung 4 lässt das bewusst offen).
- [V0.1 misst Annahme, nicht Verständnis] → bewusst so: Dogfood-Zyklus (Selbstreflexion im Gedächtnis) ist das Instrument; die Signal-Events sind die Vorstufe für spätere Evidenz.
- [CSP-/Build-Frust im Webview] → konventionelles Vite-Setup mit statischem CSS, kein Toolkit, keine HMR-Sonderwege.

## Open Questions

- Kalibrierung des `comment_ignored`-Zeitraums (Vorschlag 10 Minuten) – im Dogfood am Gefühl, ändert keinen Vertrag.
- Ob V0.2-Cooldown vor oder nach dem dritten Regler-Stufe kommt – erst die Dogfood-Daten zeigen das.

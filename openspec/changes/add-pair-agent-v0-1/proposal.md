# Proposal

## Why

Aktuelle Coding-Agents sind auf Durchsatz optimiert: ein Prompt, autonomer Lauf, dahinter Diff-Abnicken. Der Mensch wird zum müden Reviewer, baut kein mentales Modell mehr auf und verliert Verständnis und Lerneffekt (SPEC.md §1). Die Forschung liefert zugleich den Bauplan für die Alternative: Ein Explanation Gate (Teach-back vor Übernahme) halbierte die Fehlerquote in einer Wartungsaufgabe ohne KI (77 % → 39 %, N=78); kontingentes Scaffolding schlägt feste Support-Level; Selbst-Erklären ist einer der robustesten Lernmechanismen (RESEARCH.md). Die Lücke "verständnis-erhaltende Zusammenarbeit mit einem Agent" ist fast unbesetzt – die Modelle können es bereits, es fehlt der Rahmen.

## What Changes

- Neues Projekt (Greenfield): **pAIr V0.1** – ein Agent als Pair-Programming-Partner als dünne VS Code-Extension mit hostunabhängigem Core (Entscheidung 7 in SPEC.md §8).
- **Pair-Sessions** auf Kommando: Session-basiert (ein Arbeitsvorhaben = eine Session), keine Daueranwesenheit.
- **Chat-Sidebar mit geteiltem Kontext**: Der Agent sieht aktive Datei, Selection und Terminal-Output des Menschen.
- **Save-Trigger**: Beim Speichern/Verlassen einer Region darf der Agent kurz kommentieren – als Feststellung bei objektiven Bugs, als Frage mit aufklappbarer Antwort (Reveal) bei Design-Zweifeln.
- **Lautstärkeregler mit zwei Stufen** (Still / Kollege) im UI, mechanisch erzwungen als Erlaubnis-Grenze; Dosierung über ein Proaktivitäts-Gate (max. 1 Kommentar pro Save, Modell urteilt innerhalb eines Budgets).
- **Verhaltenssignal-Logging** ab Tag 1 (Reveal-Klick, ausbleibende Reaktion, Moduswechsel) ins Session-Protokoll – in V0.1 noch ungenutzt, Datenbasis für die spätere Kontingenz.
- **Multi-Provider**: provider-neutraler API-Layer; V0.1 mit direktem Mistral-Adapter (weitere Provider und OpenRouter als spätere Drop-ins), Modell konfigurierbar.
- **Markdown-Gedächtnis**: Session-Protokoll + wachsende Notiz-Datei, beim Session-Start geladen, für den Menschen les- und editierbar.
- Bewusst nicht in V0.1: Solo-Modus + Re-Sync (Autonomie-Phasen), echte Deixis (Highlights, Gutter-Marker), Was-wäre-wenn-Annotationen, kontingente Regler-Mechanik, Erklär-Modus/Teach-back (SPEC.md §9).

## Capabilities

### New Capabilities

- `pair-mode`: Der enge Dialog am gemeinsamen Code – Session-Handling, geteilter Kontext, Save-Trigger-Kommentare mit Reveal-Fragen, Lautstärkeregler mit Proaktivitäts-Gate, Vorschläge statt Writes, Markdown-Gedächtnis und Multi-Provider als tragende Teile der Interaktion (entspricht SPEC.md §4, MVP-Schnitt in §9).

### Modified Capabilities

- (keine – das Projekt hat noch keine Specs)

## Impact

- **Neuer Code**: VS Code-Extension (TypeScript, React + Tailwind im Webview, CSP-bewusst gebaut) plus hostunabhängiger Core (Session, Markdown-Gedächtnis, Provider-Layer, Proaktivitäts-Gate, Trigger-Engine, Signal-Log) hinter einem schmalen Host-Adapter. Repo-Struktur und Build-Pipeline entstehen neu.
- **Abhängigkeiten**: VS Code Extension API; React, Vite, Tailwind (Webview); API-Zugang zu mind. einem LLM-Provider bzw. OpenRouter (API-Key, konfigurierbar).
- **Keine bestehenden Systeme betroffen**: Das Repo enthält außer den Konzeptdokumenten (SPEC.md, RESEARCH.md) noch keinen Code; es gibt keine API-Verbraucher, die gebrochen werden könnten.
- **Nutzer-Perspektive**: Ein persönliches Einzelnutzer-Werkzeug ohne Team-Features (Entscheidung 3); Installation per lokaler vsix, kein Marketplace nötig.

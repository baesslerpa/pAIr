# Tasks

## 1. Projektgerüst

- [ ] 1.1 Repo-Struktur anlegen (`src/core/`, `src/extension/`, `src/webview/`), TypeScript + `tsconfig` + `package.json` mit Scripts; Verify: `tsc --noEmit` kompiliert fehlerfrei
- [ ] 1.2 Vitest für `src/core` einrichten und ersten Beispiel-Test laufen lassen; Verify: `vitest run` grün
- [ ] 1.3 Extension-Scaffold: Manifest (Commands "Session starten"/"Session beenden", Settings für Provider/Modell), esbuild-Bundle, Debug-Launch-Config; Verify: Extension startet im Debug-Host (F5)
- [ ] 1.4 Webview-Pipeline: Vite + React + Tailwind, CSP-konformes HTML (statisch gebundelte CSS-Datei, kein Inline-Script), Theme-Token auf `--vscode-*`-Variablen + `vscode-light`/`vscode-dark`-Klassen; Verify: Sidebar rendert im Debug-Host ohne CSP-Fehler, Theme-Wechsel hell/dunkel greift

## 2. Markdown-Gedächtnis und Session (Core)

- [ ] 2.1 Gedächtnis-Modul: `memory.md` (wachsende Notizen) lesen und schreiben, Session-Protokolle unter `sessions/<datum>.md` anlegen; Verify: Unit-Tests für Laden und Persistieren
- [ ] 2.2 Session-Lifecycle: Start lädt Notizen in den Kontext und beginnt das Protokoll, Stopp persistiert und beendet sauber; Verify: Unit-Tests (Start lädt Inhalt, Stopp schreibt Datei)
- [ ] 2.3 Signal-Log: Events `mode_changed` (mit Richtung), `reveal_clicked`, `comment_ignored` (Zeitraum konfigurierbar, Default 10 min), `comment_posted` mit Zeitstempel ins Session-Protokoll; Verify: Unit-Tests pro Event-Typ
- [ ] 2.4 Menschliche Bearbeitung: Dokumentieren, dass `.pair/memory.md` direkt editierbar ist (Kommentar-Header in der Datei); Verify: geänderte Notizen erscheinen im Kontext der nächsten Session (Test)

## 3. Provider-Layer (Core)

- [ ] 3.1 Provider-Interface + direkter Mistral-Adapter, API-Key über SecretStorage, Provider/Modell aus Settings; Verify: Unit-Test mit Mock-HTTP (kein echter API-Call)
- [ ] 3.2 Fehlende/ungültige Konfiguration ergibt verständliche Meldung statt stillen Fehlschlag; Verify: Unit-Test für beide Fehlerfälle

## 4. Host-Adapter (Extension)

- [ ] 4.1 `HostPort`-Interface Core-seitig definieren (onSave, getActiveFile, getSelection, getRecentTerminalOutput, zeigeChatNachricht, getReglerStufe) + Test-Dummy; Verify: Core-Tests laufen gegen den Dummy
- [ ] 4.2 VS Code-Implementierung: Save-Events, aktive Datei, Selection, Terminal-Output über Shell-Integration mit bewusster Degrade (Hinweis in der Sidebar, kein Fehler); Verify: Smoke-Test im Debug-Host (Save, Selection, Terminal ohne Shell-Integration)
- [ ] 4.3 Session-Kommandos und Regler-Zustand an den Core verdrahten; Verify: Kommandos starten/beenden eine echte Session im Debug-Host

## 5. Proaktivitäts-Gate und Trigger-Engine (Core)

- [ ] 5.1 Gate als zentrale Entscheidungsfunktion (Modus, Trigger, Budget → schweigen/sprechen) mit hart erzwungenen Invarianten: Still → niemals ungefragt, max. 1 Kommentar pro Save; Verify: Unit-Tests für jede Invariante
- [ ] 5.2 Trigger-Engine: Save/Region-Verlassen feuert das Gate mit Kontext aus dem HostPort; Verify: Unit-Tests gegen Test-Dummy
- [ ] 5.3 Kommentar-Generierung: Form-Regeln im Prompt (objektiver Fehler → Feststellung; Design-Zweifel → Frage-mit-Reveal), Antwort über Provider-Layer; Verify: Unit-Test mit Mock-Provider zur Form-Einhaltung + ein echter Kommentar im Debug-Host

## 6. Chat-Sidebar und Frage-Karte (Webview)

- [ ] 6.1 Konversationsview: Nachrichten und Kommentar-Karten mit klickbarem file:line-Verweis, Anzeige des geteilten Kontexts (aktive Datei, Selection-Status, Terminal-Status); Verify: gerenderte Karten, Link springt zur Stelle
- [ ] 6.2 Frage-Karte mit Reveal: Antwort verborgen, Klick deckt auf und löst `reveal_clicked`; Verify: Klick erzeugt Protokolleintrag
- [ ] 6.3 Lautstärkeregler in der Sidebar (Still/Kollege, Default Still), Wechsel wirkt sofort und loggt `mode_changed`; Verify: UI-Wechsel ändert das Gate-Verhalten sichtbar
- [ ] 6.4 Vorschlags-Rendering: mehrzeilige Vorschläge als Copy-Paste-Blöcke ohne Auto-Apply-Aktion; Verify: Übernahme erfordert manuelle Handlung

## 7. End-to-End und Dogfood-Start

- [ ] 7.1 vsix bauen und lokal in VS Code installieren; Verify: Extension läuft im Alltagstool ohne Debug-Host
- [ ] 7.2 E2E-Durchlauf gegen die Spec-Szenarien: Session starten (Notizen geladen), Kollege-Modus, Save erzeugt Kommentar bzw. Frage-Karte, Still schaltet ab, Session stoppt mit Protokoll; Verify: beobachtbares Verhalten deckt jedes Szenario in specs/pair-mode/spec.md
- [ ] 7.3 README: Installation (vsix), Konfiguration (Provider, API-Key), Hinweis zur Shell-Integration (D10-Degrade); Verify: Anleitung von null aus nachvollziehbar
- [ ] 7.4 Dogfood-Reflexionsformat: Abschnitts-Vorlage im `memory.md` (Was hat der Agent kommentiert? Was war gut? Was hat genervt?); Verify: Vorlage nach erster echter Session ausgefüllt

## Workflow follow-up

- Change nach Review archivieren (`/opsx-archive`), dann Ergebnis prüfen.
- Aus design.md Open Questions im Dogfood kalibrieren: `comment_ignored`-Zeitraum, Reihenfolge V0.2-Cooldown vs. dritte Regler-Stufe.

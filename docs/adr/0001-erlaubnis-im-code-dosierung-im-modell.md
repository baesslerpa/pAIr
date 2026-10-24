---
status: accepted
---

# Erlaubnis erzwingt der Code, dosiert das Modell

Der Lautstärke-Regler setzt mechanisch, was der Agent *darf*: In Still gibt es niemals einen ungefragten Kommentar, im Kollege-Modus höchstens ein Kommentar pro Save — garantiert im Code, unabhängig davon, was das Modell tut. Innerhalb dieser Erlaubnis-Grenze entscheidet das Modell an einer zentralen Stelle (Proaktivitäts-Gate), ob sich ein Kommentar lohnt und welche Form er hat; eine nur per Prompt vereinbarte Grenze kann ein gesprächiges Modell aushebeln, und vollständige Determinierung versagt, weil das Dosierungs-Urteil sozial-nuanciert ist.

## Considered Options

- `if mode == kollege`-Checks verteilt im Trigger-Code — verworfen: Die spätere Kontingenz-Mechanik (Cooldown, Stufenleiter) würde zum Umbau an vielen Stellen.
- Volle Modellautonomie mit Prompt-Regeln — verworfen: Keine garantierte Invariante, Klippy-Risiko.
- Vollständig deterministische Regeln (immer kommentieren bei Save) — verworfen: Das "lohnt sich das hier?"-Urteil ist der Kern des Produkts.

## Consequences

Spätere Ausbaustufen (Cooldown nach Interaktion, kontingente Stufenleiter) verändern nur Parameter des Gates — keine neuen Codepfade. Das Gate ist deshalb eine einzelne, zentrale Naht im hostunabhängigen Core.

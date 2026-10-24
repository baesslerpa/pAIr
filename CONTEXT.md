# pAIr

Ein Agent als Pair-Programming-Partner: Dialog statt Auftrags-Takt, der Mensch bleibt der handelnde Part. Dieser Kontext umfasst die Interaktionsform des Tools (nicht die Editor-Technik).

## Language

### Interaktion

**Session**:
Ein Chat-Gespräch mit dem Agenten. Beginnt implizit mit einem neuen Chat und endet mit ihm — kein explizites Start/Stopp.
_Avoid_: Meeting, Arbeitsvorhaben, Daueranwesenheit

**Pair-Kontext**:
Was der Agent vom Menschen sieht: die aktive Datei und deren Selection, sofern vorhanden.
_Avoid_: Workspace-Kontext, Wissensraum

**Lautstärke-Regler**:
Der UI-Schalter, der festlegt, was der Agent darf. Zwei Stufen in V0.1: Still und Kollege.
_Avoid_: Modus-Wahl, Proaktivitäts-Setting

**Still**:
Regler-Stufe, in der der Agent ausschließlich auf explizite Fragen antwortet. Standard-Stellung bei Session-Start — Initiative ist nie der Default.

**Kollege**:
Regler-Stufe, in der der Agent zusätzlich von selbst Kommentare abgeben darf — in V0.1 nur beim Save-Trigger.

**Save-Trigger**:
Das Speichern einer Datei als Ereignis, das dem Agenten eine Kommentier-Gelegenheit gibt — höchstens ein Kommentar pro Save.
_Avoid_: Auto-Review, Region-Trigger

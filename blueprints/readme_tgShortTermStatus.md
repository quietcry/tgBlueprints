# Kurzzeitstatus

## Was ist dieses Blueprint?

Dieses Blueprint verwaltet einen Kurzzeitstatus mit mehreren konfigurierbaren Status-Optionen. Jeder Status kann mehrere Buttons (Input Boolean oder Switch) haben, die automatisch synchronisiert werden. Wenn ein Button aktiviert wird, werden alle anderen Buttons des gleichen Status ebenfalls aktiviert. Zusätzlich kann für jeden Status ein Timer konfiguriert werden, der nach Ablauf automatisch alle Buttons dieses Status wieder deaktiviert. Der aktive Status-Text wird in einer Input Text Entity angezeigt.

[comment]: <> (This blueprint manages a short-term status with multiple configurable status options. Each status can have multiple buttons (Input Boolean or Switch) that are automatically synchronized. When one button is activated, all other buttons of the same status are also activated. Additionally, a timer can be configured for each status that automatically deactivates all buttons of this status after expiration. The active status text is displayed in an Input Text entity.)

## Features

- **Mehrere Status-Optionen:** Konfiguration von bis zu 5 verschiedenen Status-Optionen
- **Button-Synchronisation:** Alle Buttons innerhalb eines Status werden automatisch synchronisiert
- **Timer-Unterstützung:** Optionaler Timer für jeden Status, der Buttons nach Ablauf automatisch deaktiviert
- **Status-Anzeige:** Aktiver Status-Text wird in einer konfigurierbaren Input Text Entity angezeigt
- **Flexible Entitäten:** Unterstützung für Input Boolean und Switch Entitäten
- **Unabhängige Status:** Status-Optionen beeinflussen sich nicht gegenseitig
- **Robuste Fehlerbehandlung:** Das Blueprint behandelt fehlende Entitäten elegant

## Konfiguration

### Erforderliche Eingaben

- **Input Text - Status:** Die Input Text Entity, in der der aktive Status-Text angezeigt wird
- **Debug-Modus:** Optionaler Debug-Modus für Fehlersuche (standardmäßig deaktiviert)

### Status-Konfiguration (bis zu 5 Status)

Für jeden Status können Sie konfigurieren:

- **Entitäten:** Eine oder mehrere Input Boolean oder Switch Entitäten, die diesen Status repräsentieren
- **Timer:** Optionale Timer Entity für automatische Deaktivierung
- **Status-Text:** Text, der angezeigt wird, wenn dieser Status aktiv ist
- **Timer-Dauer:** Dauer des Timers (Standard: 30 Minuten)

## Funktionsweise

### Button-Aktivierung

1. Wenn ein Button aktiviert wird, werden alle anderen Buttons des gleichen Status automatisch synchronisiert und ebenfalls aktiviert
2. Die Synchronisation erfolgt ohne erneutes Auslösen der Automation

### Timer-Funktionalität

1. Wenn ein Timer für einen Status konfiguriert ist und ein Button aktiviert wird, startet der Timer automatisch
2. Wenn der Timer abläuft (Status wird `idle`), werden alle Buttons dieses Status automatisch deaktiviert
3. Wenn ein Button manuell deaktiviert wird, wird der zugehörige Timer (falls aktiv) abgebrochen

### Status-Text Anzeige

1. Das Blueprint prüft alle konfigurierten Status
2. Für jeden Status wird der Zustand des ersten Buttons geprüft
3. Wenn ein Button aktiv ist (`on`), wird der zugehörige Status-Text verwendet
4. **Wichtig:** Wenn mehrere Status gleichzeitig aktiv sind, wird der Status-Text des **zuletzt gefundenen** aktiven Status angezeigt
5. Wenn kein Status aktiv ist, bleibt der Status-Text leer

### Technische Details

- **Modus:** Das Blueprint verwendet `mode: queued`, um mehrere gleichzeitige Trigger korrekt zu verarbeiten
- **Trigger:** Reagiert auf State-Änderungen von Input Boolean, Switch und Timer Entitäten
- **Variablen:** Verwendet Jinja-Templates für komplexe Logik und Datenverarbeitung
- **Robustheit:** Das Blueprint behandelt fehlende oder ungültige Entitäten elegant und verursacht keine Fehler

## Verwendung

### Beispiel-Konfiguration

**Status 1:**
- Entitäten: `input_boolean.kurzzeitstatus_1`, `switch.status_1_alternative`
- Timer: `timer.kurzzeitstatus_1`
- Status-Text: "Kurzzeitstatus 1"
- Timer-Dauer: "00:05:00" (5 Minuten)

**Status 2:**
- Entitäten: `input_boolean.kurzzeitstatus_2`
- Timer: `timer.kurzzeitstatus_2`
- Status-Text: "Kurzzeitstatus 2"
- Timer-Dauer: "00:03:00" (3 Minuten)

### Verhalten bei mehreren aktiven Status

Wenn mehrere Status gleichzeitig aktiv sind, wird der Status-Text des **zuletzt in der Konfiguration gefundenen** aktiven Status angezeigt. Dies ermöglicht eine Priorisierung durch die Reihenfolge in der Konfiguration.

## Debug-Modus

Wenn der Debug-Modus aktiviert ist, werden zusätzliche Informationen in einer Persistent Notification angezeigt:
- Aktueller `state_data` (welcher Button/Timer ausgelöst hat)
- `setted_states` (alle konfigurierten Status)
- `observed_buttons` und `observed_timers` (beobachtete Entitäten)
- Trigger-Informationen

## Hinweise

- Das Blueprint verwendet `mode: queued`, um mehrere gleichzeitige Trigger korrekt zu verarbeiten
- Buttons werden synchronisiert, ohne die Automation erneut auszulösen
- Das Blueprint ist robust und behandelt fehlende Entitäten elegant
- Der Status-Text wird nur aktualisiert, wenn er sich tatsächlich geändert hat
- Timer werden automatisch gestartet und gestoppt, wenn Buttons aktiviert/deaktiviert werden


# Extended Person States - Home Assistant Blueprint

Ein Home Assistant Automation Blueprint, der erweiterte Statuswerte für Person-Entities bereitstellt. Erweitert die Standard-Person-Statuswerte (home, away, not_home) um bis zu 5 konfigurierbare Zustände.

## Übersicht

Dieser Blueprint ermöglicht es, detaillierte Statusinformationen für Personen in Home Assistant zu verwalten. Basierend auf verschiedenen Bedingungen (Person-Status, verknüpfte Geräte, Zeitpläne) wird ein erweiterter Statuswert berechnet und in eine Text-Entity geschrieben.

## Funktionen

- **Bis zu 5 konfigurierbare Zustände** pro Person
- **Bedingte Statuslogik** basierend auf:
  - Basisstatus der Person (z.B. "home", "away")
  - Verknüpfte Geräte (device_tracker, binary_sensor)
  - Zeitpläne (schedule entities)
- **Text-Input-Integration**: Optionale Text-Entities können an den Status angehängt oder diesen ersetzen
- **Aktivitäts-Tracking**: Optionaler Boolean-Helper zur Aktivierung/Deaktivierung des Trackings
- **Debug-Modus**: Umfangreiche Debug-Ausgaben für Fehlersuche
- **Personennamen-Integration**: Optional kann der Personennamen dem Status vorangestellt werden

## Installation

1. Kopiere die Datei `tgPersonState.yaml` in dein Home Assistant Blueprint-Verzeichnis:
   ```
   config/blueprints/automation/tgPersonState/
   ```

2. In Home Assistant:
   - Gehe zu **Einstellungen** → **Automatisierungen & Szenen**
   - Klicke auf **Automatisierung erstellen**
   - Wähle **Blueprint verwenden**
   - Wähle **Extended person states**

## Konfiguration

### Pflichtfelder

- **Person entity [mandatory]**: Die Person-Entity, für die der erweiterte Status berechnet werden soll (sollte mit einem Tracker verknüpft sein)

### Optionale Felder

#### Grundkonfiguration

- **is activ button [optional]**: Ein Boolean-Helper (input_boolean), der anzeigt, ob der Status für diese Person getrackt wird. Wenn dieser Button auf "off" steht, wird der Status auf "inactive" gesetzt.
- **state text-entity [optional]**: Eine Text-Entity (input_text), in die der berechnete Status geschrieben wird
- **add person name to state [default = false]**: Wenn aktiviert, wird der Personennamen dem Status vorangestellt (z.B. "Max!homeoffice")
- **debug mode**: Aktiviert detaillierte Debug-Ausgaben als persistente Benachrichtigung

#### Text-Inputs

- **Text inputs [optional]**: Liste von Text-Input-Entities, deren Werte zum Status hinzugefügt werden
- **Text input mode**:
  - `append`: Text-Inputs werden mit "|" als Separator an den bestehenden Status angehängt
  - `replace`: Text-Inputs ersetzen den Status komplett

#### Status-Konfiguration (Status 1-5)

Jeder der 5 Status kann individuell konfiguriert werden:

- **State [optional]**: Der Name des Status (z.B. "homeoffice", "schlafen", "kochen")
- **refers to state [optional]**: Der Basisstatus der Person, der erfüllt sein muss (z.B. "home"). Mehrere Werte können mit ";" getrennt werden (z.B. "home;away")
- **devices [optional]**: Verknüpfte Geräte (device_tracker oder binary_sensor), die einen on/off-Status liefern. Der Status wird gesetzt, sobald mindestens eines der Geräte "on" oder "home" ist
- **Schedule [optional]**: Ein Schedule-Entity, der aktiviert sein muss, damit dieser Status gesetzt wird

## Funktionsweise

### Status-Berechnung

Die Automation prüft die Status-Bedingungen in folgender Reihenfolge:

1. **Aktivitäts-Button**: Wenn konfiguriert und auf "off", wird der Status auf "inactive" gesetzt
2. **Status 1-5**: Die Status werden sequenziell geprüft:
   - Basisstatus der Person wird validiert (falls konfiguriert)
   - Zeitplan wird geprüft (falls konfiguriert)
   - Verknüpfte Geräte werden geprüft (mindestens eines muss "on" oder "home" sein)
   - Der letzte erfüllte Status wird gesetzt
3. **Text-Inputs**: Je nach Modus werden Text-Inputs angehängt oder ersetzen den Status
4. **Personennamen**: Falls aktiviert, wird der Name dem Status vorangestellt

### Trigger

Die Automation wird ausgelöst durch:

- Home Assistant Start
- Täglich um 00:00:00
- State-Changes von beobachteten Entities (Person, Activity-Button, verknüpfte Geräte, Text-Inputs, Schedules)

### Performance-Optimierung

Die Automation beobachtet nur die tatsächlich konfigurierten Entities, um unnötige Ausführungen zu vermeiden.

## Beispiele

### Beispiel 1: Home Office-Erkennung

```
Person entity: person.max
State text-entity: input_text.max_status

Status 1:
  State: homeoffice
  refers to state: home
  devices: binary_sensor.laptop_docked
```

Ergebnis: Wenn Max zu Hause ist und der Laptop angedockt ist, wird der Status "homeoffice" gesetzt.

### Beispiel 2: Schlaf-Status mit Zeitplan

```
Person entity: person.max
State text-entity: input_text.max_status

Status 1:
  State: schlafen
  refers to state: home
  devices: binary_sensor.bett_belegt
  schedule: schedule.nachtruhe
```

Ergebnis: Wenn Max zu Hause ist, das Bett belegt ist und der Nachtruhe-Zeitplan aktiv ist, wird der Status "schlafen" gesetzt.

### Beispiel 3: Mit Personennamen und Text-Input

```
Person entity: person.max
State text-entity: input_text.max_status
add person name to state: true
Text inputs: input_text.max_aktivitaet
Text input mode: append
```

Ergebnis: Wenn der Status "homeoffice" ist und `input_text.max_aktivitaet` den Wert "Programmieren" hat, wird "Max!homeoffice|Programmieren" gesetzt.

## Debug-Modus

Wenn der Debug-Modus aktiviert ist, werden bei jeder Ausführung detaillierte Informationen als persistente Benachrichtigung angezeigt:

- Aktueller Status
- Prüfung der Basisstatus-Bedingung
- Prüfung des Zeitplans
- Prüfung aller verknüpften Geräte
- Auslöser-Informationen

Dies ist besonders hilfreich bei der Einrichtung und Fehlersuche.

## Hinweise

- Die Automation läuft im `single` Modus, d.h. sie wird nicht parallel ausgeführt
- Leere oder nicht konfigurierte Status werden übersprungen
- Der Status wird nur aktualisiert, wenn sich der Wert ändert
- Mehrere Basisstatus können mit ";" getrennt werden (z.B. "home;away")

## Technische Details

- **Domain**: automation
- **Mode**: single
- **Template-Engine**: Jinja2
- **Unterstützte Entity-Typen**:
  - person
  - device_tracker
  - binary_sensor
  - input_boolean
  - input_text
  - schedule

## Fehlerbehebung

### Status wird nicht aktualisiert

1. Prüfe, ob der Activity-Button (falls konfiguriert) auf "on" steht
2. Aktiviere den Debug-Modus und prüfe die Ausgaben
3. Stelle sicher, dass die verknüpften Geräte existieren und gültige States haben
4. Prüfe, ob der Basisstatus der Person mit der Konfiguration übereinstimmt

### Status ist immer "inactive"

- Prüfe den Activity-Button: Er muss auf "on" stehen oder nicht konfiguriert sein
- Prüfe im Debug-Modus, welche Bedingungen nicht erfüllt sind

### Text-Entity wird nicht aktualisiert

- Stelle sicher, dass eine gültige `input_text` Entity ausgewählt wurde
- Prüfe, ob die Entity existiert und beschreibbar ist

## Version

Aktuelle Version: Siehe Datei-Header

## Lizenz

Dieses Blueprint ist für den persönlichen Gebrauch erstellt worden.


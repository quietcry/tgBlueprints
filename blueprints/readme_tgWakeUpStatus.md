# tgWakeUpStatus

## What is this blueprint for?

Dieses Blueprint überwacht Weckzeiten (wie eine Handyuhr) und setzt einen Status-Text, wenn alle konfigurierten Bedingungen erfüllt sind. Es unterstützt optionale Vorlaufzeiten für Aktionen vor dem Wecken (z.B. Bad heizen, Kaffee kochen) und kann Weckzeiten automatisch von GMT in die lokale Zeitzone konvertieren.

[comment]: <> (This blueprint monitors wake-up times (like a phone alarm) and sets a status text when all configured conditions are met. It supports optional pre-wake durations for actions before waking (e.g., heating bathroom, making coffee) and can automatically convert wake-up times from GMT to local timezone.)

## Features

- **Weckzeit-Überwachung**: Überwacht Weckzeiten aus einem Sensor (wie eine Handyuhr)
- **GMT/Lokalisierung**: Automatische Erkennung und Konvertierung von GMT-Zeiten in die lokale Zeitzone
- **Weckzeit-Validierung**: Ignoriert Weckzeiten die über 0:00 Uhr liegen
- **Personen-Zuordnung**: Status kann einer Person zugeordnet werden, deren Status einem Muster entsprechen muss
- **Bedingungslisten**: 
  - Liste von Entities die "on" sein müssen
  - Liste von Entities die "off" sein müssen
- **Vorlaufzeit**: Optionale Vorlaufzeit vor dem Wecken für Aktionen (z.B. Bad heizen, Kaffee kochen)
- **Status-Anzeige**: Setzt einen konfigurierbaren Status-Text wenn alle Bedingungen erfüllt sind

## Configuration

### Required Inputs

- **Input Text - Status**: Die Input Text Entity wo der Status-Text angezeigt wird
- **Person**: Person Entity die überwacht werden soll
- **Person Status Pattern**: Muster für Person-Status (z.B. "home", "not_home", oder Jinja-Template)
- **Weckzeit-Sensor**: Sensor Entity die die Weckzeit enthält (kann GMT oder Lokalzeit sein)

### Optional Inputs

- **Vorlaufzeit**: Optionale Vorlaufzeit vor dem Wecken (z.B. "01:00:00" für 1 Stunde)
- **Entities die ON sein müssen**: Liste von Input Boolean oder Switch Entities die "on" sein müssen
- **Entities die OFF sein müssen**: Liste von Input Boolean oder Switch Entities die "off" sein müssen
- **Status-Text**: Text der angezeigt wird, wenn alle Bedingungen erfüllt sind (Standard: "Wecker aktiv")
- **Vorlauf-Aktionen**: Optionale Aktionen die vor dem Wecken ausgeführt werden (z.B. Bad heizen, Kaffee kochen)
- **Debug-Modus**: Aktiviert Debug-Ausgaben für Fehlersuche

### How it works

1. **Weckzeit-Überwachung**: Das Blueprint prüft minütlich ob die Weckzeit erreicht ist
2. **GMT-Konvertierung**: Wenn die Weckzeit als GMT erkannt wird, wird sie automatisch in die lokale Zeitzone konvertiert
3. **Weckzeit-Validierung**: Weckzeiten die über 0:00 Uhr liegen werden ignoriert
4. **Bedingungsprüfung**: Wenn die Weckzeit erreicht ist (oder innerhalb der Vorlaufzeit), werden folgende Bedingungen geprüft:
   - Person-Status entspricht dem konfigurierten Muster
   - Alle "on"-Entities sind "on"
   - Alle "off"-Entities sind "off"
5. **Status-Setzung**: Wenn alle Bedingungen erfüllt sind, wird der Status-Text gesetzt
6. **Vorlauf-Aktionen**: Wenn innerhalb der Vorlaufzeit (aber noch nicht Weckzeit), werden die Vorlauf-Aktionen ausgeführt
7. **Weck-Aktionen**: Wenn die Weckzeit erreicht ist, werden die Weck-Aktionen ausgeführt

### Weckzeit-Format

Die Weckzeit kann in folgenden Formaten im Sensor angegeben sein:
- `HH:MM` (z.B. "07:30")
- `HH:MM:SS` (z.B. "07:30:00")

Das Blueprint erkennt automatisch ob die Zeit als GMT angegeben ist (über Sensor-Attribute wie `time_zone` oder `timezone`) und konvertiert sie in die lokale Zeitzone.

### Person Status Pattern

Das Person Status Pattern kann sein:
- Einfacher String-Vergleich: `"home"`, `"not_home"`, etc.
- Jinja-Template: `"{{- states('person.tommy') == 'home' }}"`

### Notes

- Das Blueprint verwendet `mode: queued` um mehrere Trigger korrekt zu verarbeiten
- Weckzeiten die über 0:00 Uhr liegen werden ignoriert (z.B. 25:00 wird ignoriert)
- Die Vorlaufzeit wird von der Weckzeit rückwärts berechnet
- Wenn keine Vorlaufzeit konfiguriert ist, werden Vorlauf-Aktionen nicht ausgeführt
- Wenn Bedingungen nicht mehr erfüllt sind, wird der Status-Text auf "-" zurückgesetzt

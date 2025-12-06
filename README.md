# TG Blueprints für Home Assistant

Eine Sammlung von Automatisierungs-Blueprints für Home Assistant.

## Verfügbare Blueprints

### 1. TG Schichtplan - Schichtprüfung

Prüft, ob heute eine der ausgewählten Schichten im Schichtplan aktiv ist. Basierend auf der TG Schichtplan Card.

**Funktionen:**
- Prüft automatisch, ob heute eine Schicht aktiv ist
- Unterstützt Zeitfenster pro Schicht (bis zu 2 Zeiträume)
- Berücksichtigt nur Schichten mit `statusRelevant: true`
- Automatische Timer-Verwaltung für nächste Prüfung
- Status-Entity wird automatisch aktualisiert
- Debug-Modus für detaillierte Informationen

**Abhängigkeiten:**
- [TG Schichtplan Card](https://github.com/quietcry/tgShiftSchedule-card) muss installiert sein
- `input_text` Entity für den Schichtplan
- Optional: `input_text` Entity für die Konfiguration (`<plan_entity>_config`)
- Optional: `input_text` Entity für den Status (`<plan_entity>_status`)
- Optional: `timer` Entity für automatische Prüfungen

### 2. Extended Person States

Erweitert Person-Entities um konfigurierbare Status-Zustände. Prüft verschiedene Bedingungen und gibt den letzten zutreffenden Status zurück.

**Funktionen:**
- Bis zu 5 konfigurierbare Status-Zustände
- Unterstützung für Device Tracker und Binary Sensors
- Schedule-Integration für zeitbasierte Zustände
- Text-Input-Integration (append oder replace)
- Optional: Person-Name im Status
- Debug-Modus für detaillierte Informationen

**Abhängigkeiten:**
- Person-Entity (vorzugsweise mit Tracker verknüpft)
- Optional: `input_boolean` für Aktivitäts-Button
- Optional: `input_text` für Status-Ausgabe
- Optional: `input_text` Entities für zusätzliche Informationen
- Optional: `schedule` Entities für zeitbasierte Zustände

### 3. TG Heating - Heizungssteuerung basierend auf Person-Status

Heizungssteuerung basierend auf dem Status von Personen. Trennt Logik und Konfiguration, ermöglicht flexible Heizungssteuerung ohne feste Kalenderabhängigkeit.

**Funktionen:**
- Heizungssteuerung basierend auf Person-Status
- Trennung von Logik und Konfiguration
- Unterstützung für Wochenpläne
- Flexible Befehlskonfiguration (JSON)
- Debug-Modus für Nachvollziehbarkeit
- Unterstützung für verschiedene Thermostat-Typen (inkl. Homematic)

**Abhängigkeiten:**
- Person-Entities oder Text-Helfer mit Status
- Heizungsgeräte (Thermostate)
- Optional: Wochenplan-Timer
- Optional: State-File für Zwischenspeicherung

**Hinweis:** Das Blueprint verwendet JSON-Strings für die Konfiguration. Ein JSON-Validator wird empfohlen.

### 4. TG Sprinkler - Gartenbewässerung

Automatisierte Gartenbewässerung mit erweiterten Sicherheitsfunktionen und flexibler Konfiguration.

**Funktionen:**
- Automatische Bewässerung basierend auf Zeitplan
- Feuchtigkeitssensor-basierte Bewässerung
- Wetterabhängige Bewässerung (Regenerkennung)
- Manueller Modus für direkte Kontrolle
- Sicherheitsabschaltung bei kritischen Bedingungen
- Detaillierte Statusmeldungen und Benachrichtigungen

**Abhängigkeiten:**
- Home Assistant Version 2023.8 oder höher
- Mindestens ein Bewässerungsschalter
- Optional: Feuchtigkeitssensor
- Optional: Wettersensor
- Optional: Hauptschalter für Notabschaltung
- Timer für Bewässerungsdauer und Pausen

### 5. TG Delete Notification

Löscht verzögert persistente Benachrichtigungen basierend auf Schlüsselwörtern im Titel.

**Funktionen:**
- Automatisches Löschen von Benachrichtigungen
- Schlüsselwort-basierte Filterung
- Verzögertes Löschen möglich

**Abhängigkeiten:**
- Keine speziellen Abhängigkeiten

### 6. TG Reload Config

Lädt die Home Assistant Konfiguration neu, wenn ein Button aktiviert wird.

**Funktionen:**
- Neuladen der Konfiguration über Button
- Neuladen von Automatisierungen
- Einfache Bedienung

**Abhängigkeiten:**
- `input_boolean` Entity als Button

## Installation

### Ein-Klick-Installation (Empfohlen)

Klicke auf einen der folgenden Buttons, um das Blueprint direkt in Home Assistant zu importieren:

**TG Schichtplan - Schichtprüfung:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2Ftgshiftschedule.yaml)

**Extended Person States:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2FtgPersonState.yaml)

**TG Heating - Heizungssteuerung:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2FtgHeating.yaml)

**TG Sprinkler - Gartenbewässerung:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2FtgSprinkler.yaml)

**TG Delete Notification:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2FtgDeleteNotification.yaml)

**TG Reload Config:**
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fquietcry%2FtgBlueprints%2Fmain%2Fblueprints%2Fautomation%2FtgReloadConfig.yaml)

### Manuelle Installation

Falls die Buttons nicht funktionieren, kannst du die Blueprints auch manuell importieren:

1. Öffne Home Assistant
2. Gehe zu **Einstellungen** → **Automatisierungen & Szenen**
3. Klicke auf **Blueprint importieren** (oben rechts)
4. Füge eine der folgenden URLs ein:

**TG Schichtplan - Schichtprüfung:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgshiftschedule.yaml
```

**Extended Person States:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgPersonState.yaml
```

**TG Heating:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgHeating.yaml
```

**TG Sprinkler:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgSprinkler.yaml
```

**TG Delete Notification:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgDeleteNotification.yaml
```

**TG Reload Config:**
```
https://raw.githubusercontent.com/quietcry/tgBlueprints/main/blueprints/automation/tgReloadConfig.yaml
```

5. Klicke auf **Blueprint importieren**

### Manueller Download

1. Lade die gewünschte YAML-Datei von GitHub herunter
2. Speichere sie in deinem `config/blueprints/automation/` Verzeichnis
3. Starte Home Assistant neu
4. Die Blueprints erscheinen automatisch in der Blueprint-Liste

## Verwendung

### TG Schichtplan - Schichtprüfung

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **TG Schichtplan - Schichtprüfung**
4. Konfiguriere:
   - **Plan-Entity**: Die `input_text` Entity mit deinem Schichtplan
   - **Schichtplanprüfung-Timer**: Optional, ein Timer für automatische Prüfungen
   - **Aktion wenn Schicht aktiv**: Was soll passieren, wenn heute eine Schicht aktiv ist?
   - **Aktion wenn keine Schicht**: Was soll passieren, wenn heute keine Schicht aktiv ist?
   - **Debug-Modus**: Aktivieren für detaillierte Logs

### Extended Person States

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **Extended Person States**
4. Konfiguriere:
   - **Person entity**: Die Person-Entity, die überwacht werden soll
   - **State text-entity**: Optional, `input_text` Entity für Status-Ausgabe
   - **is activ button**: Optional, `input_boolean` für Aktivitäts-Status
   - **Status 1-5**: Konfiguriere bis zu 5 Status-Zustände mit:
     - State-Name
     - Basis-Status (z.B. "home")
     - Verknüpfte Devices (Tracker/Binary Sensors)
     - Schedule (optional)
   - **Text inputs**: Optional, zusätzliche Text-Entities
   - **Text input mode**: append oder replace
   - **Debug mode**: Aktivieren für detaillierte Logs

### TG Heating - Heizungssteuerung

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **Heating control based by person state**
4. Konfiguriere:
   - **Person-state entities**: Personen oder Text-Helfer mit Status
   - **Heating devices**: Alle zu steuernden Heizungsgeräte
   - **Default command**: Standard-Konfiguration als JSON
   - **Sequence**: Befehlspriorität (z.B. "off,boost,T,WP,P")
   - **Weekplans**: Optional, Wochenpläne als JSON
   - **Command line**: Personen-spezifische Befehle als JSON
   - **Switch 1-5**: Optional, Schalter mit Befehlen
   - **Debug-mode**: Aktivieren für detaillierte Logs

**Wichtig:** Verwende einen JSON-Validator (z.B. [JSONLint](https://jsonlint.com/)) für die Konfiguration!

### TG Sprinkler - Gartenbewässerung

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **tgSprinkler**
4. Konfiguriere:
   - **Bewässerungsschalter**: Der Schalter für die Bewässerung
   - **Timer**: Timer für Bewässerungsdauer und Pausen
   - **Feuchtigkeitssensor**: Optional
   - **Wettersensor**: Optional
   - **Hauptschalter**: Optional, für Notabschaltung
   - **Bewässerungsdauer**: Dauer in Sekunden
   - **Pausendauer**: Pause zwischen Bewässerungen
   - **Feuchtigkeitslimit**: Grenzwert für Feuchtigkeit

### TG Delete Notification

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **tgDeleteNotification**
4. Konfiguriere:
   - **Schlüsselwörter**: Wörter im Titel, die gelöscht werden sollen
   - **Verzögerung**: Optional, Verzögerung vor dem Löschen

### TG Reload Config

1. Erstelle eine neue Automatisierung
2. Wähle **Blueprint verwenden**
3. Suche nach **Reload Config**
4. Konfiguriere:
   - **Button**: `input_boolean` Entity als Trigger
   - **Automations**: Liste von Automatisierungen, die neu geladen werden sollen

## Beispiele

### Beispiel: TG Schichtplan mit Benachrichtigung

```yaml
# Automatisierung erstellt mit Blueprint "TG Schichtplan - Schichtprüfung"
# Konfiguration:
# - Plan-Entity: input_text.arbeitszeiten
# - Aktion wenn Schicht aktiv: Benachrichtigung senden
# - Aktion wenn keine Schicht: Status auf "frei" setzen
```

### Beispiel: Person State mit Homeoffice-Erkennung

```yaml
# Automatisierung erstellt mit Blueprint "Extended Person States"
# Konfiguration:
# - Person: person.tommy
# - Status 1: "homeoffice"
#   - Basis-Status: "home"
#   - Devices: binary_sensor.homeoffice_detector
# - State text-entity: input_text.tommy_status
```

## Updates

Um ein Blueprint zu aktualisieren:

1. Importiere das Blueprint erneut über die GitHub-URL (Methode 1)
2. Home Assistant fragt, ob die bestehende Version überschrieben werden soll
3. Bestätige die Aktualisierung

**Hinweis:** Bestehende Automatisierungen, die das Blueprint verwenden, werden automatisch aktualisiert.

## Support

Bei Fragen oder Problemen:
- Öffne ein Issue auf [GitHub](https://github.com/quietcry/tgBlueprints/issues)
- Oder kontaktiere den Entwickler

## Lizenz

Siehe [LICENSE](LICENSE) Datei für Details.


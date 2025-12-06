# TG Blueprints für Home Assistant

Eine Sammlung von Automatisierungs-Blueprints für Home Assistant, die mit der TG Schichtplan Card zusammenarbeiten.

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

## Installation

### Methode 1: Direkter Import über GitHub (Empfohlen)

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

5. Klicke auf **Blueprint importieren**

### Methode 2: Manueller Download

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


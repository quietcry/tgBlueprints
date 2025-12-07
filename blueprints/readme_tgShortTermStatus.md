# Kurzzeitstatus

## What is this blueprint for?

Dieses Blueprint verwaltet einen Kurzzeitstatus mit mehreren konfigurierbaren Status-Optionen. Jeder Status kann mehrere Buttons (Input Boolean oder Switch) haben, die automatisch synchronisiert werden. Wenn ein Button aktiviert wird, werden alle anderen Buttons des gleichen Status ebenfalls aktiviert. Zusätzlich kann für jeden Status ein Timer konfiguriert werden, der nach Ablauf automatisch alle Buttons dieses Status wieder deaktiviert. Der aktive Status-Text wird in einer Input Text Entity angezeigt.

[comment]: <> (This blueprint manages a short-term status with multiple configurable status options. Each status can have multiple buttons (Input Boolean or Switch) that are automatically synchronized. When one button is activated, all other buttons of the same status are also activated. Additionally, a timer can be configured for each status that automatically deactivates all buttons of this status after expiration. The active status text is displayed in an Input Text entity.)

## Features

- **Multiple Status Options:** Configure up to 5 different status options
- **Button Synchronization:** All buttons within a status are automatically synchronized
- **Timer Support:** Optional timer for each status that automatically deactivates buttons after expiration
- **Status Display:** Active status text is displayed in a configurable Input Text entity
- **Flexible Entities:** Support for both Input Boolean and Switch entities
- **Independent Status:** Status options do not influence each other

## Configuration

### Required Inputs

- **Input Text - Status:** The Input Text entity where the active status text will be displayed

### Status Configuration (up to 5 statuses)

For each status you can configure:

- **Entitäten:** One or more Input Boolean or Switch entities that represent this status
- **Timer:** Optional Timer entity for automatic deactivation
- **Status-Text:** Text that will be displayed when this status is active
- **Timer-Dauer:** Duration of the timer (default: 30 minutes)

### How it works

1. When a button is activated, all other buttons of the same status are also activated
2. If a timer is configured, it is started when a button is activated
3. When the timer expires, all buttons of that status are automatically deactivated
4. The status text of the active status is displayed in the Input Text entity
5. If multiple statuses are active, the first one found is displayed

## Notes

- The blueprint uses `mode: queued` to handle multiple triggers correctly
- Buttons are synchronized without re-triggering the automation
- The blueprint is robust and handles missing entities gracefully


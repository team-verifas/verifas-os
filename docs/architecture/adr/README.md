# Architecture Decision Records

Dieses Verzeichnis enthält die Architecture Decision Records für
Verifas OS.

Die deutsche Fassung eines ADR ist maßgeblich. Die englische Fassung ist
eine inhaltlich gleichwertige Übersetzung.

## Statusmodell

- **Proposed:** vorgeschlagen und noch nicht verbindlich
- **Accepted:** durch die zuständige Instanz angenommen
- **Rejected:** geprüft und abgelehnt
- **Withdrawn:** vor der Entscheidung zurückgezogen
- **Deprecated:** weiterhin dokumentiert, aber nicht mehr empfohlen
- **Superseded:** durch ein neueres ADR ersetzt

Ein angenommenes ADR bedeutet nicht automatisch, dass seine Umsetzung
abgeschlossen oder forensisch validiert ist.

## ADR-Index

| ADR | Titel | Klasse | Status | Vorschlagsdatum | Entscheidung |
|---|---|---:|---|---|---|
| [ADR-0001 DE](ADR-0001-sichere-blockgeraete-de.md) / [EN](ADR-0001-safe-block-devices-en.md) | Sichere Behandlung unbekannter Blockgeräte | D2 | Proposed | 2026-11-07 | offen |

## Gründungsphase

Verifas befindet sich derzeit in der Gründungsphase. Eine unabhängige
forensische und sicherheitsbezogene Prüfung ist noch nicht vollständig
verfügbar.

Entscheidungen und Entwicklungsartefakte müssen dieses Restrisiko sichtbar
ausweisen. Sie dürfen ohne erforderliche unabhängige Prüfung nicht als
stabil oder forensisch validiert bezeichnet werden.

## Prüfverfahren für ADR-0001

ADR-0001 unterliegt als D2-Entscheidung einer regulären öffentlichen
Kommentierungsfrist von 14 Kalendertagen. Die Frist beginnt erst, wenn der
vollständige Entwurf über einen öffentlich zugänglichen Pull Request zur
Prüfung bereitsteht.

Während der Prüfphase sind ausschließlich klar gekennzeichnete,
nicht produktive Experimente mit synthetischen Testdaten zulässig.

# M1 – Verifas OS Bootstrap

## Status

Experimenteller Entwicklungsprototyp. Nicht freigegeben und nicht für echte
Untersuchungen, Beweismittel oder produktive Einsätze vorgesehen.

## Zweck

Erstellung einer minimalen, bootfähigen Debian-13-Live-ISO als technische
Grundlage für Verifas OS.

## Problem

Vor Branding, forensischen Standardwerten und Werkzeug-Integration muss
nachgewiesen werden, dass ein nachvollziehbarer ISO-Build mit Debian Stable
und live-build grundsätzlich funktioniert.

## Entwicklungsbasis

- Debian 13 Stable (trixie)
- Zielarchitektur: amd64
- Buildsystem: live-build aus den offiziellen Debian-Paketquellen
- Buildumgebung: separate Debian-13-VM
- Drittwerkzeuge: keine
- Forensikwerkzeuge: noch keine

## Forensische Anforderungen

- ausschließlich dokumentierte Paketquellen verwenden
- Buildversionen und Paketmanifest erfassen
- ISO-Prüfsumme erzeugen
- Buildschritte nachvollziehbar dokumentieren
- Entwicklungsimage sichtbar als nicht validiert kennzeichnen

## Risiken und Einschränkungen

- Paketstände können sich innerhalb von Debian Stable ändern
- reproduzierbare Bit-für-Bit-Builds sind noch nicht nachgewiesen
- Secure Boot ist noch nicht validiert
- BIOS- und UEFI-Start müssen getrennt getestet werden
- das Image besitzt noch keine forensisch sicheren Standardwerte

## Teststrategie

1. ISO-Build muss ohne Fehler abgeschlossen werden.
2. SHA-256-Prüfsumme muss erzeugt werden.
3. ISO muss in VMware mit UEFI starten.
4. ISO muss zusätzlich mit BIOS-Firmware getestet werden.
5. Debian-Version und Architektur müssen im Live-System geprüft werden.
6. Paketmanifest und Buildinformationen müssen erhalten bleiben.

## Dokumentation

Buildbefehle, Werkzeugversionen, Prüfsummen, Testergebnisse und bekannte
Einschränkungen werden im Repository oder in den zugehörigen Buildartefakten
dokumentiert.

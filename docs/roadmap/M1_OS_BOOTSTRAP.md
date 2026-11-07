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

## Buildnachweis 001

- Datum: 7. November 2026
- Entwicklungsstand: M1 Bootstrap
- Konfigurationscommit: a664d10
- Build-ID: 20261107T101302Z
- ISO: verifas-os-dev-amd64.hybrid.iso
- Größe: 385875968 Bytes
- SHA-256: 98232333a9dd67c6713c84a0057588a0fd50ac92f9eed4fac2111e4b00b6ef37
- Debian: 13.7 (trixie)
- Architektur: amd64
- BIOS-Start: erfolgreich
- UEFI-Start: erfolgreich
- Secure Boot: noch nicht getestet
- Freigabe: Entwicklungsprototyp, nicht für echte Beweismittel

Die ISO wurde aus dem noch nicht committeten Arbeitsstand erzeugt, dessen
unveränderte Konfiguration anschließend als Commit a664d10 gesichert wurde.
Vor einer Veröffentlichung muss der Build erneut aus einem sauberen,
eindeutig referenzierten Commit erzeugt werden.

## Abschluss M1

Der Bootstrap wurde am 7. November 2026 mit Buildnachweis 002 aus dem
sauberen Commit `cd156ff9e768ba8e9450ed73b5653e8c0719c4dc` erneut erzeugt und
erfolgreich mit BIOS sowie UEFI getestet.

Der vollständige Nachweis einschließlich Buildprotokoll und Paketmanifest
befindet sich unter `docs/build-evidence/m1-002/`.

M1 bestätigt ausschließlich die technische Live-Build-Grundlage. Das Image
ist noch nicht für forensische Untersuchungen oder echte Beweismittel
freigegeben.

## Abschluss M1

Der Bootstrap wurde am 7. November 2026 mit Buildnachweis 002 aus dem
sauberen Commit `cd156ff9e768ba8e9450ed73b5653e8c0719c4dc` erneut erzeugt und
erfolgreich mit BIOS sowie UEFI getestet.

Der vollständige Nachweis einschließlich Buildprotokoll und Paketmanifest
befindet sich unter `docs/build-evidence/m1-002/`.

M1 bestätigt ausschließlich die technische Live-Build-Grundlage. Das Image
ist noch nicht für forensische Untersuchungen oder echte Beweismittel
freigegeben.

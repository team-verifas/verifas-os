# M1 – Buildnachweis 002

## Status

Erfolgreicher Entwicklungs- und Nachweisbuild des Verifas-OS-Bootstraps.

Das erzeugte Image ist ein nicht freigegebener Entwicklungsprototyp. Es ist
nicht für echte Untersuchungen, Beweismittel oder produktive Einsätze
vorgesehen.

## Buildgrundlage

- Datum: 7. November 2026
- Quellbranch: `experiment/m1-live-build-bootstrap`
- Quellcommit: `cd156ff9e768ba8e9450ed73b5653e8c0719c4dc`
- Arbeitsverzeichnis vor dem Build: unverändert
- Buildsystem: Debian 13.7 (trixie), amd64
- live-build: `20250505+deb13u1`
- Build-ID: `20261107T150836Z`

## ISO-Artefakt

- Ursprünglicher Name: `verifas-os-dev-amd64.hybrid.iso`
- Transportname: `verifas-os-dev-amd64-cd156ff.hybrid.iso`
- Größe: `385875968` Bytes
- SHA-256: `202743cbd2425301755ac8d5f3f1d7b541d78dbb5182260ae26f0641a29696f8`
- Volume-ID: `VERIFAS_OS_DEV`
- Publisher-ID: `VERIFAS PROJECT`

Die SHA-256-Prüfsumme wurde nach der Übertragung von der Build-VM auf den
Windows-Host erneut berechnet und erfolgreich verglichen.

## Bootnachweis

Die ISO enthält getrennte El-Torito-Booteinträge für BIOS und UEFI.

- VMware-Start mit UEFI: erfolgreich
- VMware-Start mit BIOS: erfolgreich
- erkannte Architektur: `x86_64`
- gestartetes System: Debian GNU/Linux 13.7 (trixie)
- Image-Typ: Live-System
- Secure Boot: nicht getestet

## Paketmanifest

- Datei: `package-manifest.txt`
- Einträge: `288`
- SHA-256: `bc256ccf98ac7a191730e405586dd8ddaeb98694b857607981b6f5cd742c50ac`

## Buildprotokoll

- Datei: `build-log.txt.gz`
- ursprüngliche Datei: `m1-final-20261107T150836Z.log`
- SHA-256 des unkomprimierten Protokolls: `871f341e42b6942fb5abe447dba3f2d40ee22d0b1ab582c0fba43c3349335e0c`
- Buildabschluss: erfolgreich

## Reproduzierbarkeit

Der Nachweisbuild wurde aus einem sauberen und eindeutig referenzierten
Git-Commit erzeugt. Die technische Herkunft des Images ist damit
nachvollziehbar.

Bit-für-Bit-Reproduzierbarkeit ist noch nicht nachgewiesen. Der vorherige
Build hatte bei identischer Größe eine abweichende SHA-256-Prüfsumme.
Zeitstempel und weitere variable Builddaten werden in einem späteren
Meilenstein kontrolliert.

## Einschränkungen

- keine validierten forensischen Standardwerte
- kein validierter Schreibschutz für Beweismitteldatenträger
- keine forensischen Werkzeuge integriert
- keine Secure-Boot-Validierung
- keine Freigabe für echte Beweismittel

Dieser Build bestätigt ausschließlich den erfolgreichen, nachvollziehbaren
Debian-Live-Bootstrap für BIOS und UEFI.

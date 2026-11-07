# ADR-0001: Sichere Behandlung unbekannter Blockgeräte

- Status: Proposed
- Entscheidungsklasse: D2
- Datum des Vorschlags: 2026-11-07
- Entscheidungsdatum: offen
- Verantwortliche Person(en): Daniel Demele
- Entscheidungsinstanz: Projektleitung während der Gründungsphase; später TSC
- Betroffene Komponenten: Verifas OS
- Ersetzt: –
- Ersetzt durch: –
- Verknüpfte Anforderungen: M2-FB-001 bis M2-FB-010
- Verknüpfte Risiken / Threat Model: unbeabsichtigte Schreibzugriffe auf mögliche Beweismittel
- Verknüpfte Issues / Pull Requests: offen

## Kontext und Problemstellung

Allgemeine Linux-Systeme können Blockgeräte erkennen, untersuchen und
automatisch einbinden. Dateisystemtreiber, Desktopdienste und andere
Hintergrundprozesse können dabei Schreibzugriffe auslösen.

Selbst eine als read-only angeforderte Dateisystemeinbindung ist nicht für
jedes Dateisystem ausreichend. Beispielsweise können Journal-Recovery oder
dateisystemspezifische Mechanismen zusätzliche Schutzoptionen erfordern.

Verifas OS benötigt deshalb eine sichere Standardbehandlung für unbekannte
Blockgeräte. Ein softwarebasierter Schutz kann das Risiko reduzieren, ist
aber kein gleichwertiger Ersatz für einen validierten Hardware-Write-Blocker.

## Entscheidungstreiber

- Schutz möglicher Originalbeweismittel
- keine impliziten Schreibzugriffe
- sichtbare und überprüfbare Schutzstatus
- Fail-closed-Verhalten
- nachvollziehbare Geräteidentifikation
- testbare und reproduzierbare Konfiguration
- spätere Erweiterbarkeit für kontrollierte Erfassungsziele
- klare Darstellung technischer Grenzen

## Betrachtete Optionen

### Option A – Debian-Standardverhalten beibehalten

Das unveränderte Debian-Verhalten wird verwendet.

Vorteile:

- keine zusätzliche Implementierung
- hohe Kompatibilität mit Standardwerkzeugen

Nachteile:

- kein projektweit definierter Beweismittelschutz
- zukünftige Desktoppakete können Automount einführen
- Schutzstatus und Fehlerverhalten bleiben uneinheitlich

### Option B – Nur Automount deaktivieren

Automatische Dateisystemeinbindungen werden deaktiviert. Weitere
Schutzmaßnahmen erfolgen manuell.

Vorteile:

- geringer Implementierungsumfang
- verhindert einen häufigen unbeabsichtigten Zugriffspfad

Nachteile:

- kein technischer Read-only-Status des Blockgeräts
- privilegierte Prozesse können weiterhin schreiben
- manueller Mount kann unsichere Optionen verwenden

### Option C – Mehrschichtiger softwarebasierter Schutz

Automount wird deaktiviert. Unbekannte Blockgeräte werden zunächst als
potenzielle Quellen behandelt. Ein logischer Read-only-Status wird so früh
wie praktikabel angefordert und anschließend geprüft. Dateisysteme werden
nur über freigegebene, dateisystemspezifische Read-only-Profile eingebunden.

Vorteile:

- mehrere voneinander unabhängige Schutzschichten
- sichtbare Verifikation statt bloßer Annahme
- kontrollierbares Fehlerverhalten
- automatisierbar und mit virtuellen Datenträgern testbar

Nachteile:

- komplexer als reine Automount-Deaktivierung
- mögliche Race Conditions bei der Geräteerkennung
- Root oder fehlerhafte Kernelkomponenten können den Schutz umgehen
- Dateisysteme benötigen unterschiedliche Mount-Profile

### Option D – Ausschließlich Hardware-Write-Blocker

Mögliche Quellen dürfen nur über validierte Hardware-Write-Blocker
angeschlossen werden.

Vorteile:

- stärkere Trennung vom untersuchenden Betriebssystem
- etablierter Schutzansatz für physische Originalmedien

Nachteile:

- nicht für alle Gerätetypen verfügbar
- für virtuelle Entwicklungs- und Testmedien ungeeignet
- Hardware und Firmware müssen separat validiert werden
- Betriebssystem benötigt trotzdem sichere Standardwerte

## Entscheidung

Vorgeschlagen wird Option C als Verifas-OS-Baseline.

Für reale physische Originalmedien wird zusätzlich ein geeigneter und für
den konkreten Gerätetyp validierter Hardware-Write-Blocker vorausgesetzt,
sobald ein entsprechender Workflow freigegeben wird.

Die Baseline umfasst folgende Regeln:

1. Automatische Einbindungen unbekannter Datenträger sind deaktiviert.
2. Unbekannte Geräte werden standardmäßig als mögliche Quellen behandelt.
3. Ein logischer Read-only-Status wird vor einer Nutzung angefordert.
4. Der angeforderte Status wird technisch geprüft.
5. Eine unklare oder fehlgeschlagene Prüfung blockiert die weitere Nutzung.
6. Generische Read-only-Mounts gelten nicht automatisch als sicher.
7. Einbindungen verwenden freigegebene dateisystemspezifische Profile.
8. Nicht unterstützte Dateisysteme bleiben standardmäßig ungemountet.
9. System-, Quell- und Zieldatenträger werden ausdrücklich unterschieden.
10. Die Freigabe eines beschreibbaren Ziels ist ein separater,
    privilegierter und protokollierter Vorgang.
11. Gerätebezeichnungen wie `/dev/sdb` gelten nicht als stabile Identität.
12. Schutzmechanismen und ihre Grenzen werden sichtbar protokolliert.

## Begründung

Option C bietet eine sinnvolle mehrschichtige Baseline, ohne einen
softwarebasierten Schutz fälschlich als physische Schreibblockade
darzustellen.

Option A und B erfüllen die Anforderungen an überprüfbaren Schutz und
explizites Fehlerverhalten nicht. Option D bleibt für reale Originalmedien
relevant, kann jedoch die notwendigen sicheren Betriebssystemstandards
nicht ersetzen.

## Folgen

### Positive Folgen

- unbekannte Datenträger erhalten einen sicheren Ausgangszustand
- automatische Dateisystemzugriffe werden reduziert
- Schutzstatus und Fehler werden überprüfbar
- spätere Erfassungswerkzeuge können auf einer definierten Baseline aufbauen
- Tests sind mit synthetischen virtuellen Datenträgern möglich

### Negative Folgen und Zielkonflikte

- zusätzlicher Implementierungs- und Testaufwand
- manche Datenträger oder Treiber unterstützen den angeforderten Status nicht
- Komfortfunktionen allgemeiner Desktop-Systeme werden eingeschränkt
- Dateisystemprofile müssen einzeln entwickelt und validiert werden
- beschreibbare Zieldatenträger benötigen einen getrennten Freigabeprozess

### Rest-Risiken und Einschränkungen

- Root kann softwarebasierten Schreibschutz grundsätzlich aufheben
- Kernel-, Treiber- und Firmwarefehler bleiben außerhalb vollständiger Kontrolle
- Geräte können fehlende oder falsche Identifikatoren liefern
- zwischen Erkennung und Schutzaktivierung können Race Conditions bestehen
- Read-only-Mountoptionen besitzen dateisystemspezifische Grenzen
- virtuelle Tests ersetzen keine Hardwarevalidierung
- der Ansatz ist noch nicht für echte Beweismittel freigegeben

## Forensische Auswirkungen

Die Entscheidung reduziert das Risiko unbeabsichtigter Veränderungen,
garantiert jedoch nicht allein die Unverändertheit eines Originalmediums.

Vor und nach späteren Erfassungs- oder Testvorgängen bleiben unabhängige
Integritätsprüfungen erforderlich. Ein einzelner Hashwert belegt nur die
Übereinstimmung der verglichenen Bytefolgen und nicht automatisch eine
ordnungsgemäße Herkunft oder Erfassung.

## Sicherheits- und Datenschutzfolgen

Der Schutzmechanismus benötigt begrenzte privilegierte Systemfunktionen.
Diese dürfen nicht als allgemeine Root-Schnittstelle verfügbar gemacht
werden.

Protokolle erfassen technische Geräteinformationen, dürfen aber keine
unnötigen Fallinhalte, Zugangsdaten oder Geheimnisse enthalten.

## Reproduzierbarkeit und Lieferkette

Die Konfiguration wird deklarativ im Repository gepflegt und über den
bestehenden live-build-Prozess in das Image übernommen.

Verwendete Debian-Pakete, Konfigurationsdateien, Skripte, Buildinformationen
und Testergebnisse werden versioniert oder durch Prüfsummen referenziert.

## Kompatibilität, Migration und Rücknahme

Die Entscheidung betrifft zunächst ausschließlich Entwicklungsimages.

Eine Rücknahme erfolgt durch Entfernung der M2-Konfiguration und Neubau des
Images. Bereits erzeugte Testnachweise bleiben erhalten und werden nicht
rückwirkend umgeschrieben.

## Test- und Validierungsstrategie

Die Validierung verwendet ausschließlich synthetische virtuelle
Testfestplatten mit bekannten Inhalten und Prüfsummen.

Mindestens geprüft werden:

1. Erkennung ohne automatische Einbindung.
2. korrekte Abgrenzung des Systemdatenträgers.
3. Anforderung und Verifikation des Read-only-Status.
4. kontrolliertes Scheitern eines Schreibversuchs.
5. dateisystemspezifische Read-only-Einbindung.
6. identische Vorher-/Nachher-Prüfsummen.
7. Verhalten bei fehlenden Geräteidentifikatoren.
8. Verhalten bei nicht unterstützten Dateisystemen.
9. Verhalten bei fehlgeschlagener Schutzaktivierung.
10. BIOS- und UEFI-Regressionstest des erzeugten Images.

## Dokumentationsbedarf

Erforderlich sind:

- M2-Anforderungen,
- dieses ADR in deutscher und englischer Fassung,
- technische Beschreibung der Schutzschichten,
- Testdatenträger-Spezifikation,
- reproduzierbare Testanleitung,
- Test- und Buildnachweise,
- bekannte Einschränkungen,
- spätere Betriebsanweisung für reale Medien.

## Einwände und abweichende Positionen

Noch keine dokumentiert.

Während der Gründungsphase fehlt eine unabhängige forensische und
sicherheitsbezogene Prüfung. Daraus entsteht ein ausdrücklich dokumentiertes
Restrisiko. Das Ergebnis darf nicht als stabil oder forensisch validiert
bezeichnet werden.

## Umsetzungsplan

1. aktuellen M1-Ist-Zustand mit synthetischem Datenträger beobachten.
2. Automount- und Gerätezugriffspfade inventarisieren.
3. mögliche Schutzmechanismen experimentell vergleichen.
4. Race Conditions und Fehlerzustände untersuchen.
5. minimale deklarative Konfiguration implementieren.
6. automatisierbare negative und positive Tests erstellen.
7. neuen referenzierten ISO-Build erzeugen.
8. Ergebnisse und Restrisiken dokumentieren.

Produktive Umsetzung und Freigabe erfolgen erst nach Annahme des ADR.
Explorative Tests bleiben ausdrücklich nicht produktiv und verwenden keine
realen Beweismittel.

## Referenzen

- `docs/roadmap/M2_FORENSIC_BASELINE.md`
- Verifas Forensic Principles
- Verifas OS Specification
- Verifas Development Guidelines
- Verifas Project Governance
- Linux-Kernel- und Debian-Dokumentation, vor Implementierung zu referenzieren

## Änderungshistorie

| Datum | Status | Änderung |
|---|---|---|
| 2026-11-07 | Proposed | Erstentwurf |

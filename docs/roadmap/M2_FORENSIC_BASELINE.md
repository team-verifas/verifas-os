# M2 – Forensische Grundkonfiguration

## Status

Planungsentwurf. Noch nicht implementiert, validiert oder freigegeben.

## Entscheidungsklasse

D2 – Die geplanten Änderungen betreffen den Schutz möglicher Beweismittel,
Schreibzugriffe, Vertrauensgrenzen und forensisch relevante Protokollierung.

Vor der Implementierung sind ein Proposal, mindestens ein ADR sowie eine
dokumentierte Risiko- und Testbewertung erforderlich.

## Zweck

M2 schafft eine minimale forensische Sicherheitsbasis für Verifas OS.

Ein angeschlossener Datenträger darf nicht allein durch seine Erkennung,
Dateisystemprüfung oder Darstellung unbeabsichtigt verändert werden. Das
System muss unbekannte Datenträger zunächst als mögliche Beweismittel
behandeln und einen unsicheren Zustand sichtbar ablehnen.

## Problem

Allgemeine Desktop- und Linux-Systeme können Datenträger automatisch
einbinden, Metadaten aktualisieren, Hilfsverzeichnisse anlegen oder Dienste
auf erkannte Dateisysteme zugreifen lassen.

Solche Zugriffe können den ursprünglichen Zustand eines möglichen
Beweismittels verändern. Eine spätere Prüfsumme kann diese vorherige
Veränderung nicht rückgängig machen.

## Leitprinzipien

1. Preserve first.
2. Verify always.
3. Analyze second.
4. Document everything.
5. Unsicherheit führt zu einem sichtbaren, sicheren Fehlerzustand.
6. Softwarebasierter Schreibschutz wird nicht als Ersatz für einen
   validierten physischen Write-Blocker dargestellt.

## Geltungsbereich

M2 umfasst:

- Erkennung angeschlossener Blockgeräte,
- Verhinderung automatischer Dateisystemeinbindungen,
- sichere Behandlung unbekannter Datenträger,
- Trennung von System-, Quell- und Zieldatenträgern,
- kontrollierte schreibgeschützte Einbindung,
- Prüfung des tatsächlich wirksamen Schreibschutzstatus,
- grundlegende Protokollierung,
- Tests ausschließlich mit synthetischen Datenträgern.

## Nicht-Ziele

M2 umfasst noch nicht:

- Untersuchung echter Beweismittel,
- vollständige forensische Erfassung,
- Fallverwaltung und Chain-of-Custody-Anwendung,
- grafische Untersuchungsoberfläche,
- Dateisystem- oder Inhaltsanalyse,
- Speicher-, Mobil- oder Cloud-Forensik,
- Freigabe bestimmter Hardware-Write-Blocker,
- allgemeine Aussage gerichtlicher Verwertbarkeit,
- produktive Freigabe von Verifas OS.

## Vertrauensmodell

Datenträger werden logisch in folgende Rollen eingeteilt:

- **System:** Datenträger, von dem Verifas OS gestartet wurde.
- **Unbekannt:** erkannt, aber noch nicht klassifiziert.
- **Quelle:** mögliches Beweismittel; grundsätzlich nicht beschreibbar.
- **Ziel:** ausdrücklich autorisiertes Medium für kontrollierte Ausgaben.

Ein unbekannter Datenträger darf nicht automatisch zum beschreibbaren Ziel
werden. Rollen dürfen nicht allein aus Gerätenamen wie `/dev/sdb` abgeleitet
werden, da sich diese zwischen Startvorgängen ändern können.

## Anforderungen

### M2-FB-001 – Kein Automount

Verifas OS darf unbekannte Datenträger und deren Partitionen nicht
automatisch einbinden.

### M2-FB-002 – Sichere Ausgangseinstufung

Neu erkannte Datenträger müssen zunächst als unbekannt und potenziell
beweisrelevant behandelt werden.

### M2-FB-003 – Keine impliziten Schreibzugriffe

Vor ausdrücklicher Klassifizierung und Autorisierung dürfen keine
Dateisystem- oder sonstigen anwendungsseitigen Schreibzugriffe erfolgen.

### M2-FB-004 – Kontrollierte Read-only-Einbindung

Eine Quelle darf nur über einen dokumentierten Ablauf schreibgeschützt
eingebunden werden. Mindestens sind die Mount-Optionen `ro`, `nosuid`,
`nodev` und `noexec` zu bewerten.

### M2-FB-005 – Wirksamkeitsprüfung

Ein angeforderter Schreibschutz darf erst als aktiv angezeigt werden, wenn
sein Status technisch geprüft wurde. Eine fehlgeschlagene oder unklare
Prüfung muss den Ablauf blockieren.

### M2-FB-006 – Trennung von Quelle und Ziel

Quelle und Ziel müssen vor einer späteren Erfassung eindeutig unterschieden
werden. Eine Verwechslung muss durch sichere Standardwerte und sichtbare
Bestätigung erschwert werden.

### M2-FB-007 – Geräteidentifikation

Die Erfassung soll mindestens dokumentieren:

- Kernel-Gerätename,
- Größe in Bytes,
- logische und physische Sektorgröße,
- Hersteller und Modell, soweit verfügbar,
- Seriennummer, soweit verfügbar,
- Bus- und Verbindungstyp,
- Partitionen und erkannte Dateisysteme,
- Read-only-Status,
- Erkennungszeitpunkt und Zeitzone.

Fehlende oder widersprüchliche Identifikatoren müssen sichtbar bleiben.

### M2-FB-008 – Protokollierung

Erkennung, Klassifizierung, Schutzversuch, Statusprüfung, Einbindung,
Aushängen, Fehler und manuelle Entscheidungen müssen nachvollziehbar
protokolliert werden.

### M2-FB-009 – Explizites Fehlerverhalten

Ein Schutzfehler, unbekannter Gerätestatus oder unerwarteter Schreibzugriff
darf nicht als Erfolg dargestellt werden. Der Vorgang muss kontrolliert
abbrechen und die Abweichung dokumentieren.

### M2-FB-010 – Keine echten Beweismittel

Entwicklung und Tests dürfen ausschließlich synthetische, öffentliche oder
ausdrücklich freigegebene Testdaten verwenden.

## Risiken

| Risiko | Auswirkung | Vorgesehene Kontrolle |
|---|---|---|
| Automatische Einbindung | Veränderung von Dateisystemdaten | Automount deaktivieren und testen |
| Falsche Gerätezuordnung | Quelle wird als Ziel behandelt | stabile Merkmale und Bestätigung |
| Nur scheinbarer Schreibschutz | unbemerkte Veränderung | Status technisch verifizieren |
| Gerät unterstützt Schutz nicht | falsches Sicherheitsversprechen | sichtbarer Fehler und Abbruch |
| Hintergrunddienst greift zu | nicht dokumentierter Zugriff | Dienste inventarisieren und minimieren |
| Wechselnde Gerätenamen | falsche Protokollzuordnung | nicht nur `/dev/sdX` verwenden |
| Testfehler beschädigt Daten | Verlust oder falscher Nachweis | ausschließlich synthetische Testmedien |
| Software-Write-Block umgehbar | Original kann verändert werden | Grenzen dokumentieren; Hardware vorsehen |

## Architekturentscheidungen

Vor Implementierung ist mindestens ein ADR erforderlich zu:

1. Richtlinie für unbekannte und potenziell beweisrelevante Blockgeräte.
2. Technische Durchsetzung und Verifikation des Read-only-Status.
3. Abgrenzung zwischen Quell- und Zieldatenträgern.
4. Protokollformat, Zeitbezug und Fehlerzustände.

Die Entscheidung muss Alternativen wie udev-Regeln, systemd-Mechanismen,
udisks-Konfiguration, Kernel-Read-only-Status und kontrollierte
Benutzerwerkzeuge bewerten.

## Teststrategie

Als Testmedium wird eine ausschließlich für M2 erzeugte virtuelle Festplatte
verwendet. Sie enthält synthetische Dateien und bekannte Prüfsummen.

Mindestens zu prüfen sind:

1. Das Testmedium wird erkannt, aber nicht automatisch eingebunden.
2. Systemdatenträger und Testmedium werden korrekt unterschieden.
3. Der Schutzstatus wird vor Verwendung geprüft.
4. Eine kontrollierte Read-only-Einbindung ist möglich.
5. Ein Schreibversuch auf die Quelle scheitert sichtbar.
6. Prüfsummen definierter Bereiche sind vor und nach dem Test identisch.
7. Aushängen und erneutes Anschließen verändern den Ausgangszustand nicht.
8. Fehlende Identifikatoren werden als unbekannt dokumentiert.
9. Schutz- und Mountfehler werden nicht als Erfolg dargestellt.
10. Das Start- und Root-Dateisystem bleibt funktionsfähig.

## Akzeptanzkriterien

M2 ist erst abgeschlossen, wenn:

- die Anforderungen und ADRs geprüft sind,
- kein Automount des synthetischen Testmediums erfolgt,
- der Read-only-Zustand technisch überprüfbar ist,
- negative Schreibtests kontrolliert scheitern,
- Vorher-/Nachher-Prüfsummen übereinstimmen,
- Fehlerfälle und bekannte Grenzen dokumentiert sind,
- BIOS- und UEFI-Buildtests weiterhin erfolgreich sind,
- ein neuer referenzierter ISO-Nachweisbuild vorliegt.

## Geplante Arbeitsschritte

1. Ist-Zustand des M1-Images beobachten und dokumentieren.
2. Hintergrunddienste und Mount-Verhalten inventarisieren.
3. ADR für die Datenträger-Sicherheitsrichtlinie erstellen.
4. synthetische M2-Testfestplatte spezifizieren.
5. Schutzmechanismen minimal implementieren.
6. positive, negative und Fehlerfälle testen.
7. neuen ISO-Nachweisbuild erstellen.
8. Einschränkungen und Testergebnisse dokumentieren.

## Freigabegrenze

Auch nach M2 bleibt Verifas OS ein Entwicklungsprototyp. Der erfolgreiche Test
mit einer virtuellen synthetischen Festplatte ist keine Validierung für reale
Beweismittel oder physische Datenträger.

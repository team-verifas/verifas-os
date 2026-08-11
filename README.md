# Verifas OS

Verifas OS is a Debian-based operating system for reproducible, transparent, and forensically sound digital investigations.

> **Development status:** Verifas OS is currently in the early development phase. Version `0.1.0-dev` is not forensically validated and must not be used for real investigations or evidentiary work.

## Mission

**Preserve. Verify. Analyze.**

Verifas OS provides the operating-system foundation of the wider Verifas open-source digital-forensics platform.

## Core principles

- Preserve first.
- Verify always.
- Analyze second.
- Document everything.

## Current milestone

The current milestone is **M1 – OS Bootstrap**.

Its objective is to establish:

- a minimal Debian 13 “trixie” live system;
- documented and controlled build procedures;
- reproducible build foundations;
- forensically safe default configurations;
- automated integrity and bootability tests.

## Repository scope

This repository will contain:

- Debian `live-build` configuration;
- package lists and package pinning;
- Verifas OS configuration and branding;
- build and verification scripts;
- automated tests;
- architecture decisions;
- build, security, validation, and release documentation.

It will not contain real evidence, case data, credentials, private keys, or other sensitive investigation material.

## Forensic assurance

No development build, test image, or unreleased artifact may be described as forensically validated unless the applicable validation, reproducibility, integrity, and release requirements have been completed and documented.

## Project status

The repository is being established according to the documented Verifas architecture, governance, security, development, and forensic-assurance principles.

No stable release is currently available.

## Contact

General project contact: `team@verifas.org`

Security vulnerabilities must not be reported through public issues. A dedicated private reporting process will be published before the first release.

## License

The project license and applicable third-party notices will be published in the repository before source code or distributable artifacts are released.
# ADR-0001: Safe Handling of Unknown Block Devices

- Status: Proposed
- Decision class: D2
- Proposal date: 2026-11-07
- Decision date: open
- Responsible person(s): Daniel Demele
- Decision authority: Project Lead during the founding phase; TSC thereafter
- Affected components: Verifas OS
- Supersedes: –
- Superseded by: –
- Linked requirements: M2-FB-001 through M2-FB-010
- Linked risks / threat model: unintended writes to potential evidence
- Linked issues / pull requests: open

## Context and Problem Statement

General-purpose Linux systems may detect, inspect, and automatically mount
block devices. Filesystem drivers, desktop services, and other background
processes may cause write operations during these activities.

Even a filesystem mount requested as read-only is not sufficient for every
filesystem. Journal recovery and filesystem-specific mechanisms may require
additional protection options.

Verifas OS therefore requires safe default handling for unknown block
devices. Software-based protection can reduce risk, but it is not an
equivalent replacement for a validated hardware write blocker.

## Decision Drivers

- protection of potential original evidence
- no implicit write operations
- visible and verifiable protection states
- fail-closed behavior
- traceable device identification
- testable and reproducible configuration
- future extensibility for controlled acquisition targets
- clear communication of technical limitations

## Considered Options

### Option A – Retain Debian Default Behavior

The unchanged Debian behavior is used.

Advantages:

- no additional implementation
- high compatibility with standard tools

Disadvantages:

- no project-wide evidence-protection policy
- future desktop packages may introduce automount behavior
- protection states and error handling remain inconsistent

### Option B – Disable Automount Only

Automatic filesystem mounts are disabled. Further protection measures are
performed manually.

Advantages:

- limited implementation effort
- prevents a common unintended access path

Disadvantages:

- no technical read-only state for the block device
- privileged processes may still write
- manual mounts may use unsafe options

### Option C – Layered Software-Based Protection

Automount is disabled. Unknown block devices are initially treated as
potential sources. A logical read-only state is requested as early as
practicable and subsequently verified. Filesystems are mounted only through
approved filesystem-specific read-only profiles.

Advantages:

- multiple independent protection layers
- visible verification instead of an unverified assumption
- controlled error behavior
- suitable for automated tests with virtual block devices

Disadvantages:

- more complex than disabling automount alone
- possible race conditions during device detection
- root or faulty kernel components can bypass the protection
- different filesystems require different mount profiles

### Option D – Hardware Write Blockers Only

Potential sources may be connected only through validated hardware write
blockers.

Advantages:

- stronger separation from the examining operating system
- established protection approach for physical original media

Disadvantages:

- not available for every device type
- unsuitable for virtual development and test media
- hardware and firmware require separate validation
- the operating system still requires safe defaults

## Decision

Option C is proposed as the Verifas OS baseline.

For real physical original media, a suitable hardware write blocker
validated for the specific device type will additionally be required once
an applicable workflow is approved.

The baseline comprises the following rules:

1. Automatic mounting of unknown storage media is disabled.
2. Unknown devices are treated as potential sources by default.
3. A logical read-only state is requested before use.
4. The requested state is technically verified.
5. An unclear or failed verification blocks further use.
6. Generic read-only mounts are not automatically considered safe.
7. Mounts use approved filesystem-specific profiles.
8. Unsupported filesystems remain unmounted by default.
9. System, source, and target devices are explicitly distinguished.
10. Enabling a writable target is a separate, privileged, and logged
    operation.
11. Device names such as `/dev/sdb` are not considered stable identities.
12. Protection mechanisms and their limitations are visibly logged.

## Rationale

Option C provides a meaningful layered baseline without falsely presenting
software-based protection as a physical write barrier.

Options A and B do not satisfy the requirements for verifiable protection
and explicit error handling. Option D remains relevant for real original
media but cannot replace the necessary safe operating-system defaults.

## Consequences

### Positive Consequences

- unknown storage devices receive a safe initial state
- automatic filesystem access is reduced
- protection states and failures become verifiable
- future acquisition tools can rely on a defined baseline
- tests can use synthetic virtual block devices

### Negative Consequences and Trade-offs

- additional implementation and testing effort
- some devices or drivers may not support the requested state
- general desktop convenience features are restricted
- filesystem profiles must be developed and validated individually
- writable target devices require a separate authorization process

### Residual Risks and Limitations

- root can generally remove software-based write protection
- kernel, driver, and firmware defects remain outside complete control
- devices may provide missing or incorrect identifiers
- race conditions may exist between detection and protection activation
- read-only mount options have filesystem-specific limitations
- virtual testing does not replace hardware validation
- the approach is not approved for real evidence

## Forensic Impact

The decision reduces the risk of unintended modification but does not, by
itself, guarantee that original media remains unchanged.

Independent integrity checks remain necessary before and after future
acquisition or test operations. A single hash value only demonstrates
equality of the compared byte sequences; it does not automatically prove
proper origin or acquisition.

## Security and Privacy Consequences

The protection mechanism requires limited privileged system functions.
These functions must not be exposed as a general-purpose root interface.

Logs record technical device information but must not contain unnecessary
case content, credentials, or secrets.

## Reproducibility and Supply Chain

The configuration is maintained declaratively in the repository and
included in the image through the existing live-build process.

Used Debian packages, configuration files, scripts, build information, and
test results are versioned or referenced by cryptographic checksums.

## Compatibility, Migration, and Reversal

The decision initially applies only to development images.

Reversal is performed by removing the M2 configuration and rebuilding the
image. Existing test evidence remains preserved and is not rewritten
retroactively.

## Test and Validation Strategy

Validation uses only synthetic virtual test disks with known contents and
checksums.

At minimum, testing covers:

1. detection without automatic mounting;
2. correct separation of the system disk;
3. request and verification of the read-only state;
4. controlled failure of a write attempt;
5. filesystem-specific read-only mounting;
6. identical before-and-after checksums;
7. behavior with missing device identifiers;
8. behavior with unsupported filesystems;
9. behavior when protection activation fails;
10. BIOS and UEFI regression testing of the generated image.

## Documentation Requirements

The following documentation is required:

- M2 requirements;
- this ADR in German and English;
- technical description of the protection layers;
- test-disk specification;
- reproducible testing instructions;
- test and build evidence;
- known limitations;
- a future operating procedure for real media.

## Objections and Dissenting Positions

None documented yet.

During the founding phase, independent forensic and security review is not
available. This creates an explicitly documented residual risk. The result
must not be described as stable or forensically validated.

## Implementation Plan

1. Observe the current M1 behavior using a synthetic storage device.
2. Inventory automount and device-access paths.
3. Experimentally compare possible protection mechanisms.
4. Investigate race conditions and failure states.
5. Implement the minimal declarative configuration.
6. Create automatable positive and negative tests.
7. Produce a new referenced ISO build.
8. Document results and residual risks.

Production implementation and approval begin only after acceptance of this
ADR. Exploratory testing remains explicitly non-production and does not use
real evidence.

## References

- `docs/roadmap/M2_FORENSIC_BASELINE.md`
- Verifas Forensic Principles
- Verifas OS Specification
- Verifas Development Guidelines
- Verifas Project Governance
- Linux kernel and Debian documentation, to be referenced before implementation

## Change History

| Date | Status | Change |
|---|---|---|
| 2026-11-07 | Proposed | Initial draft |

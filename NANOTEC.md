# Nanotec fork of KickCAT

This repository is a fork of [leducp/KickCAT](https://github.com/leducp/KickCAT), maintained by
Nanotec Electronic GmbH. Nanotec uses the KickCAT EtherCAT master stack in its software
(NanoLib). KickCAT is licensed under CeCILL-C, see [LICENSE](LICENSE); the changes made in this fork
are published here under the same licence.

This file exists on the `nanotec/main` branch only.

## Branches

| Branch | Purpose |
|---|---|
| `master` | Unmodified mirror of the upstream `master` branch. Fast-forward only, no Nanotec commits. |
| `nanotec/main` | Integration branch: upstream `master` plus the Nanotec changes. Merge only; it is never rebased or force-pushed. |
| topic branches | One branch per self-contained change, branched from `master`. Changes of general use are offered upstream as pull requests from these branches. |

## Tags

Every revision of `nanotec/main` that is shipped in a Nanotec product is tagged `nanolib-<version>`.
These tags are never moved or deleted, so the source of a shipped revision stays available.

## Changes compared with upstream

`git diff master..nanotec/main` shows the complete set of changes.

| Topic branch | Change | Offered upstream |
|---|---|---|
| `per-slave-fmmu-configuration` | `Bus`: `addFMMUConfiguration(Slave&)` queues the process data FMMUs of one slave and `addMailboxFMMUConfiguration(Slave const*)` the mailbox status FMMU of one slave (or of all slaves); `configureFMMUs()` and `configureMailboxFMMUs()` are built on them and behave as before. A master can configure the process data of a single slave, for example after the slave restarted. | Not yet |
| `eeprom-fetch-without-end-marker` | `Bus::fetchEeprom()`: the read of the SII during `init()` throws when the 16 bits word address space is exhausted and a slave has not delivered the end category (blank or damaged EEPROM). Before, the address wrapped and `init()` did not return. | Pull request [468](https://github.com/leducp/KickCAT/pull/468) |

## Changes merged upstream

These changes were developed in this fork and are part of the upstream `master` now. They are no
difference to upstream any more, and their topic branches were deleted after the merge.

| Change | Upstream pull request |
|---|---|
| The headers and sources of the master library build with MSVC: `kickcat/packed.h` (`KICKCAT_PACKED_BEGIN`/`KICKCAT_PACKED_END`) replaces `__attribute__((__packed__))`, size checks for the packed structures; `FoE::opcode::ERROR` renamed to `FoE::opcode::ERR` and the parameter `interface` renamed to `ifname` (both names are macros of the Windows API); `DOMAIN` macro of the MSVC `<math.h>` undefined in `CoE/OD.h`; `sleep()` without `clock_nanosleep()` in `OS/Windows/Time.cc`. | [leducp/KickCAT#458](https://github.com/leducp/KickCAT/pull/458) |
| `lib/master/src/dc.cc`: the DC diagnostic output prints nanosecond counts with `PRId64` instead of `%ld` (`long` has 32 bits on Windows). | [leducp/KickCAT#457](https://github.com/leducp/KickCAT/pull/457) |

## Updating from upstream

1. Fast-forward `master` to the upstream `master`.
2. Merge `master` into `nanotec/main` with a pull request inside this repository.
3. Tag the revision once it is taken over into a product.
4. Update this file: move changes that were merged upstream to "Changes merged upstream" and delete
   their topic branches.

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
| `msvc-portability` | The headers and sources of the master library build with MSVC: `kickcat/packed.h` (`KICKCAT_PACKED_BEGIN`/`KICKCAT_PACKED_END`) replaces `__attribute__((__packed__))`, size checks for the packed structures; `FoE::opcode::ERROR` renamed to `FoE::opcode::ERR` and the parameter `interface` renamed to `ifname` (both names are macros of the Windows API); `DOMAIN` macro of the MSVC `<math.h>` undefined in `CoE/OD.h`; `sleep()` without `clock_nanosleep()` in `OS/Windows/Time.cc`. | Not yet |

## Updating from upstream

1. Fast-forward `master` to the upstream `master`.
2. Merge `master` into `nanotec/main` with a pull request inside this repository.
3. Tag the revision once it is taken over into a product.

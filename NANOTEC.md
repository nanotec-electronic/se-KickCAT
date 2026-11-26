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
| `sdo-upload-abort-when-buffer-too-small` | CoE client: a segmented SDO transfer that the client gives up while the slave still waits (upload larger than the client buffer, segment with a bad toggle bit) sends an Abort SDO Transfer request to the slave before the message ends with `COE_CLIENT_BUFFER_TOO_SMALL` or `COE_SEGMENT_BAD_TOGGLE_BIT`. CoE server: an Abort SDO Transfer request received during a segmented transfer ends that transfer. | Pull request [472](https://github.com/leducp/KickCAT/pull/472) |
| `foe-rearm-timeout` | `AbstractMessage::rearmTimeout()` is public: a client that could not process the mailbox for a while restarts the timeout of a running message (of the current exchange for FoE, of the whole message for SDO) before it continues, so the pause does not end the message with `TIMEDOUT`. A message that is no longer `RUNNING` keeps its status. | Pull request [473](https://github.com/leducp/KickCAT/pull/473) |
| `sdo-abort-without-error-class` | CoE client: an Abort SDO Transfer whose code has no error class (upper 16 bits zero; ETG.1000.6 defines none) ends the SDO message with the new status `COE_ABORT_WITHOUT_ERROR_CLASS`. Before, the code was taken as the status: 0 read as `SUCCESS`, 1 as `RUNNING`, 2 as `TIMEDOUT`. The branch starts from `nanotec/main`; its commit applies to `master` unchanged. | Not yet |

## Changes merged upstream

These changes were developed in this fork and are part of the upstream `master` now. They are no
difference to upstream any more, and their topic branches were deleted after the merge.

| Change | Upstream pull request |
|---|---|
| The headers and sources of the master library build with MSVC: `kickcat/packed.h` (`KICKCAT_PACKED_BEGIN`/`KICKCAT_PACKED_END`) replaces `__attribute__((__packed__))`, size checks for the packed structures; `FoE::opcode::ERROR` renamed to `FoE::opcode::ERR` and the parameter `interface` renamed to `ifname` (both names are macros of the Windows API); `DOMAIN` macro of the MSVC `<math.h>` undefined in `CoE/OD.h`; `sleep()` without `clock_nanosleep()` in `OS/Windows/Time.cc`. | [leducp/KickCAT#458](https://github.com/leducp/KickCAT/pull/458) |
| `lib/master/src/dc.cc`: the DC diagnostic output prints nanosecond counts with `PRId64` instead of `%ld` (`long` has 32 bits on Windows). | [leducp/KickCAT#457](https://github.com/leducp/KickCAT/pull/457) |
| `Bus::fetchEeprom()`: the read of the SII during `init()` throws when the 16 bits word address space is exhausted and a slave has not delivered the end category (blank or damaged EEPROM). Before, the address wrapped and `init()` did not return. | [leducp/KickCAT#468](https://github.com/leducp/KickCAT/pull/468) |
| `Link::sendFrame()`: when the socket throws on write, the frames are cleared and the datagrams marked `SEND_ERROR` before the exception is passed on. Before, the length of the frame was left behind and the next frame was sent with both lengths, up to beyond the frame buffer. | [leducp/KickCAT#471](https://github.com/leducp/KickCAT/pull/471) |

## Updating from upstream

1. Fast-forward `master` to the upstream `master`.
2. Merge `master` into `nanotec/main` with a pull request inside this repository.
3. Tag the revision once it is taken over into a product.
4. Update this file: move changes that were merged upstream to "Changes merged upstream" and delete
   their topic branches.

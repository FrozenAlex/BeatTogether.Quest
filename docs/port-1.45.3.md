## Published branch status — 12 November 2026

This `1.45.3` branch targets Beat Saber **1.45.3_30054**. PC and Quest have
been tested together on the updated LAN UnifiedServer: both join a lobby,
start a Standard custom song, and show the failed screen. The user also tested
Quest MultiplayerExtensions. Full-song completion, late joins, reconnects and
all per-player options are not individually verified.

Runtime fixes include PC menu activation, Quest network configuration setup,
Quest platform identity injection, nonvirtual beatmap-view base calls, dedicated
server state flags, and a results window when all players fail. Validation passed:
19 dedicated-server tests, 10 HTTP tests, 7 PC tests, PC hook/signature audits,
Quest ARM64 builds, QMOD checks, and installed-library hash/load checks.

The PC BSML, SiraUtil and SongCore dependency ports are intentionally **not
published** with these branches. Reproducing the PC test requires compatible
versions of those dependencies. Quest's core pack is likewise separate; use the
versions declared by each QMOD/QPM manifest. Public BeatTogether service and
external credential verification were not validated: the LAN server disabled
authentication for the test. No general production-readiness claim is made.

The notes below preserve the port investigation/build history. Statements there
about pending tests or uncommitted changes describe earlier stages; the verified
scope above is current. The combined repository/build guide is available at
https://github.com/FrozenAlex/BeatTogether.UnifiedServer/blob/1.45.3/docs/crossplay-1.45.3.md.

---

# BeatTogether 1.45.3 port

Local test version **2.2.6**, targeting **1.45.3_30054**, with MultiplayerCore
**1.6.8**. The port adapts hook registration, config paths, UI casts, loader
exports, dependencies and the GameActivity launch name. Saved server-selection
and server-list keys are preserved.

Build MultiplayerCore first and install it into the local QPM cache with
`qpm install --cmake-build false build/libMultiplayerCore.so build/debug/libMultiplayerCore.so`
from its checkout. Then run `qpm restore`, `qpm s build`, `qpm qmod manifest`,
and `qpm qmod zip --skip_build` here. Output: `BeatTogether.qmod`.

Install the provided MultiplayerCore QMOD before this package. The local core
version is not published and has no automatic download URL. The dedicated server
must support compatibility `A31AF687`, and the master must route 1.45.3 clients
to that pool. Updating this selector alone cannot update an existing deployment.

Headset login, lobby creation/join, gameplay and PC crossplay remain unverified.
No live server was deployed as part of this source/build port.

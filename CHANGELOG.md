# Changelog

Versions track the Godot 4 SDK with a `-3x` suffix. Earlier backport history (v2.1.0-3x through v2.2.3-3x) lived in the template repo before this addon moved to its own repository; the version header comments in `CheddaBoards.gd` carry the full history.

## v2.2.5-3x — 2026-09-03

First release from this repository. Brings the backport to parity with the Godot 4 SDK v2.2.5.

### From v2.2.5
- **Direct canister reads**: `get_scoreboard()` now fetches straight from the CheddaBoards canister over HTTP (raw.icp0.io) instead of the proxy. Same JSON, same signals — nothing changes in game code. If the direct read fails (network filter, gateway hiccup), the SDK automatically retries through the proxy, and after repeated direct failures it sticks to the proxy for the rest of the session.
- **Request queue hardening**: double-dispatch guard in the queue processor, ERR_BUSY requeue in the executor, and deferred retry dispatch — fixes a race observed in the Godot 4 SDK where proxy retries could collide with in-flight requests.

### From v2.2.4 (completing the backport — nickname validation shipped in v2.2.3-3x)
- `account_upgrade_failed` now actually fires on migration failures (it was declared but never emitted).
- Linked accounts report their real provider — Apple sign-ins are no longer labelled "google".
- Device code requests seed the player's current in-game nickname, so accounts created via linking are born with the name the player chose instead of "Player_N"; on fresh accounts the chosen name is restored after migration.

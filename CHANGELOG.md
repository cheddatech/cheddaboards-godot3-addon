# Changelog

Versions track the Godot 4 SDK with a `-3x` suffix. Earlier backport history (v2.1.0-3x through v2.2.3-3x) lived in the template repo before this addon moved to its own repository; the version header comments in `CheddaBoards.gd` carry the full history.

## v2.3.2-3x — 2026-10-27

Brings the backport to parity with the Godot 4 SDK v2.3.2. No breaking changes to signals or public method signatures.

### From v2.3.1
- **Switching players resets per-player state.** `set_player_id()` with a different ID clears the cached profile, nickname, backend-existence flag, pending rename and play session, so one player's name can no longer leak into the next. `logout()` resets the same set.
- **Stale responses are dropped.** Per-player requests record which player they were sent for; a response (or queued request) for a previous player is ignored instead of being applied to the new one.

### From v2.3.0
- **Device code links survive a reload.** The pending code is saved to `user://cheddaboards_pending_link.cfg` and resumed on startup, so a player who approves on their phone after the game reloads still gets signed in.
- `login_with_device_code(force_new = false)` reuses a live pending code instead of minting a new one. Pass `true` to force a fresh code.
- New: `has_pending_device_code()`, `get_device_verification_url()`, `get_device_code_seconds_remaining()`.
- `logout()` cancels any pending device code.
- `get_game_stats()` fetches `/game` (the `/game/stats` route never existed).
- A failed achievement batch now emits `request_failed("unlock_achievement_batch", msg)` and `achievements_loaded([])` instead of going silent.

### From v2.2.7
- Score submits no longer rename the player: the nickname is only sent when one has been set.
- `change_nickname()` gates on confirmed backend existence rather than the profile cache. A name set before the player exists rides the first submit and is re-synced from the profile (max 2 attempts).
- Rename responses that don't echo the nickname fall back to the requested name.
- Profiles with no nickname are no longer backfilled with a generated default.
- `get_achievements()` reads achievements from the player profile (the old route never existed).
- Tolerant batch-achievement response parsing.

### From v2.2.6
- Identical in-flight or queued read requests are de-duplicated.
- `get_leaderboard()` default limit is now 100 (was 1000). Pass a larger limit explicitly if needed.
- Batch achievement unlocks emit `achievement_unlocked` / `achievements_loaded` again.

### Fixes specific to the 3.x port
- Startup log reported "v2.2.3-3x"; it now reads from the new `VERSION` constant.

### Not ported
- The Godot 4 DeviceCodeLogin popup and Setup Wizard.

## v2.2.5-3x — 2026-09-03

First release from this repository. Brings the backport to parity with the Godot 4 SDK v2.2.5.

### From v2.2.5
- **Direct canister reads**: `get_scoreboard()` now fetches straight from the CheddaBoards canister over HTTP (raw.icp0.io) instead of the proxy. Same JSON, same signals — nothing changes in game code. If the direct read fails (network filter, gateway hiccup), the SDK automatically retries through the proxy, and after repeated direct failures it sticks to the proxy for the rest of the session.
- **Request queue hardening**: double-dispatch guard in the queue processor, ERR_BUSY requeue in the executor, and deferred retry dispatch — fixes a race observed in the Godot 4 SDK where proxy retries could collide with in-flight requests.

### From v2.2.4 (completing the backport — nickname validation shipped in v2.2.3-3x)
- `account_upgrade_failed` now actually fires on migration failures (it was declared but never emitted).
- Linked accounts report their real provider — Apple sign-ins are no longer labelled "google".
- Device code requests seed the player's current in-game nickname, so accounts created via linking are born with the name the player chose instead of "Player_N"; on fresh accounts the chosen name is restored after migration.

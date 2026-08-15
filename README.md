# UT99 Nexgen Bans

Research/design-stage project. **No code yet** — this repo currently holds project notes only.

## Goal

Replace Nexgen's ban system, which keys on a CD-key-derived HWID, with one built on ACE's real
hardware fingerprint (`IACECheck.HWHash`). Nexgen's CD-key ID isn't a hardware identifier —
banning one player on a shared install copy currently bans every other player sharing that copy
too. Keying on ACE's actual per-machine hash instead avoids that collateral ban.

## Approach (planned, unbuilt)

A Nexgen plugin subclassing ACE's `IACEEventHandler`, reading `IACECheck.HWHash` per connecting
player, with a **deferred kick** on a ban match — not a `PreLogin` rejection, since ACE's
`HWHash` only populates a few seconds after a player joins (post-handshake).

Path confirmed viable against ACE's official UnrealScript integration package (`IACEv13.u`).
Currently evaluating whether an existing third-party Nexgen ACE plugin already covers this before
building from scratch.

## Status

Research/design phase only. See `CLAUDE.md`/`MEMORY.md` for session notes and technical
constraints as the project progresses.

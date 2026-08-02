# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this project.

## Project Goal

Replace Nexgen's flawed CD-key-based HWID ban system with one built on ACE v1.3p's real
hardware fingerprint (`HWHash`), so banning one player on a shared install copy does not
ban innocent players using the same copy.

**Status:** Research/design phase — no code written yet. Path confirmed viable: a Nexgen
plugin using ACE's official UnrealScript integration package `IACEv13.u` (subclass
`IACEEventHandler`, read `IACECheck.HWHash` per player, deferred kick on match).
Currently evaluating whether Sp0ngeb0b's existing Nexgen ACE plugin (unrealadmin.org)
already covers this before building from scratch.

## Key Files

| File | Description |
|---|---|
| *(none yet — project not started)* | Plugin source / tooling will be added here |

## External Paths & Environment

| Path | Description |
|---|---|
| `C:\UnrealTournament` | Local UT99 install (v469) with ucc.exe toolchain |
| `C:\UnrealTournament\System\IACEv13.u` | ACE's official UnrealScript integration package — the API this project builds on |
| `C:\UnrealTournament\ACEExport\IACEv13\` | Exported `.uc` class stubs from IACEv13.u (`IACECheck`, `IACEActor`, `IACEEventHandler`) |

## Server Environment

- **Host:** NFOservers (account type VPS vs game rental still unconfirmed)
- **Game:** UT99 v469e Deathmatch
- **Mods:** Nexgen, ACE v1.3p (native DLL + `IACEv13.u` interface), InstaGib+
- Nexgen bans are stored in `Nexgen.ini` under `[Nexgen.NexgenConfig]` as indexed arrays
  (`bannedClientID[n]`, `bannedIPMask[n]`, `bannedName[n]`, `banReason[n]`)

## Key Technical Constraints

- ACE's `HWHash` only populates a few seconds after a player joins (post-handshake) —
  ban enforcement must be a deferred kick, not a `PreLogin` rejection.
- Nexgen's own HWID is just a CD-key hash — never use it for identity.
- ACE logs a full per-player info block (HWID, MACHash, IP, OS, CPU) on every connect;
  log format is confirmed and parseable if an external Python tool is ever built.

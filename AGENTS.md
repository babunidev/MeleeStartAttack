# MeleeStartAttack — AI/developer instructions

**Project creator and decision authority:** Babuni.dev.

## Preserve the authored behavior

The addon connects the **player's attempted action** to melee auto-attack without automating spell choice or target selection. Its built-in design covers Paladin, Warrior and Shaman; has a Warrior stop exception; and learns extra start/stop icons per class. Do not rewrite this into a rotation optimizer or a combat assistant that chooses abilities.

Read `MeleeStartAttack.lua`, the TOC, README, concept, architecture, decisions and development journal before any change. Respect Vanilla **1.12.1 / Lua 5.0** API constraints.

## Invariants and technical caveats

- `AttackTarget()` is a toggle. Do not call it blindly on every press.
- Attack slot discovery scans **1–120**, caches a found Attack slot and checks `IsCurrentAction`; no Attack icon found means **combat-flag fallback**, which may be stale.
- `StartAutoAttack` requires a living, attackable hostile target. `StopAutoAttack` can act without a target.
- Source wraps three globals: **UseAction**, **CastSpell**, **CastSpellByName**, checking before delegating. Test coexistence/load order with other addons.
- There are **7/20/5** built-in start icons for Paladin/Warrior/Shaman, plus Warrior Intimidating Shout stop. Icons are not unique spell IDs.
- Stop matching runs **before** start. Learning a conflicting action may not override built-in behavior.
- Learned records are saved **by class**, not separately by character. Version 1.1.2 `customStartIcons` is migrated into start records with legacy placeholder names.
- Automatic `enabled` is **true on every file load** and not saved. Manual start/stop commands run regardless of enabled flag.
- Learning needs a spellbook name and icon; unidentified macro icons may leave learning armed.
- No LICENSE file exists. Do not add a license or imply permissions without the creator's explicit choice.

## Record each material change

For implementation or major discussion, preserve the creator's aim and constraints, AI options and technical contributions, iteration/corrections, what Babuni.dev accepted or deferred, exact changed paths/commits, and test outcomes with actual client versions.

Update README, CONCEPT, ROADMAP, CHANGELOG, AGENTS, docs/DEVLOG, docs/DECISIONS, docs/DESIGN, docs/ARCHITECTURE, docs/CONTENT and docs/AI_COLLABORATION as relevant. No AI proposal is approved simply because it appears in roadmap.

Do not publish raw private conversations, game account data, credentials or unrelated player information. This 2026-10-26 pass changes documentation only.

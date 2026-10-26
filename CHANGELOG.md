# MeleeStartAttack — Git-based changelog

**Creative author:** Babuni.dev. Commit timestamps prove when Git changes were recorded, **not** that a public release or successful client test happened.

## 2026-10-26 — documentation expansion

- Expanded eleven Markdown files with installation and command reference, original creative direction, class-specific icon inventory, architectural state model, interaction design, decisions, milestone journal, and human–AI attribution.
- Recovered **August 22, 29, 30 and 31** source evolution, September provider-neutral wording and October authorship/documentation work.
- Clarified per-class SavedVariables, 1.1.2 migration, hybrid Attack state, macro interception, stop-before-start precedence, default enabled setting and learned-action limitations.
- Recorded unresolved client tests and license absence.
- **Markdown-only:** no Lua, TOC or gameplay behavior changed and no client test ran.

## 2026-08-22 — first committed multi-class addon

**`ef0acb0` (“Add files via upload”)** introduced version **1.1.0** of Melee Start Attack: Paladin, Warrior and Shaman icon whitelists, Intimidating Shout stop exception, single `UseAction` wrapper, `AttackTarget()` toggle management and `/msa` on/off/status commands. README established action-attempt behavior and Vanilla 1.12.1 target.

## 2026-08-29 — custom abilities and more trigger paths

**`2eafab6` (“Refine melee start attack triggers”)** upgraded manifest to **1.1.2**, added a fifth Shaman/custom ability mapping for Lightning Strike, created per-class `customStartIcons` saving, and introduced **learning**. It also intercepted `CastSpell` and `CastSpellByName` for spellbook and macro add-ons, rather than only action buttons.

## 2026-08-30 — hybrid attack detection

**`02515ea` (“Add hybrid auto-attack detection”)** made actual Attack-action state more authoritative when an Attack button is on a bar: discover/cache the slot, use `IsCurrentAction`, otherwise fall back to a combat boolean. The manifest advanced to **1.2.1**, and manual start/stop commands appeared.

## 2026-08-31 — per-class learned start and stop

**`83f9b37` (“Add per-class learned ability controls”)** advanced version to **1.3.1**. Persistent entries now include icon, client-localized spell name and action `start`/`stop`. New commands learned stop behavior, listed entries, removed entries by name and reset the current class's saved choices. A migration preserved legacy **1.1.2** start-only icons as records with placeholder names.

## 2026-09-18 — provider-neutral client description

**`11e9387`** replaced a provider-specific Lightning Strike comment with a neutral **custom ability** description. This was a source-comment wording edit; no proof of expanded client compatibility or new spell testing.

## 2026-10-26 — author credit and first docs

**`d224a17`** updated TOC author from `Local` to Babuni.dev; **`ffc449c`** added README credit; **`b0c710a`** added the initial small documentary skeleton; **`27e8bfd`** linked it from README. The present pass deepened the history without altering source.

## What this does not prove

Git establishes changed files, dates and visible code decisions. It does not prove earlier uncommitted prototypes, who wrote every line, a successful in-game run, a custom-client support matrix or acceptance of proposed repairs.

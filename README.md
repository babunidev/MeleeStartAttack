# Melee Start Attack

**A minimal auto-attack companion for Vanilla World of Warcraft**

**Project creator:** Babuni.dev  
**Version:** 1.3.1  
**Client declared in manifest:** World of Warcraft 1.12 / interface 11200 (the project describes 1.12.1)  
**Source:** [MeleeStartAttack.lua](MeleeStartAttack.lua) · [MeleeStartAttack.toc](MeleeStartAttack.toc)  
**Verification:** source and dated Git history reviewed October 26, 2026. No in-game tests performed here.

Melee Start Attack connects **the player's intention to fight** to the game's ordinary auto-attacks. If you press a configured ability as a **Paladin, Warrior or Shaman** against a living, attackable target, the addon can start your basic melee swing **before** the original ability executes—even when a spell is on cooldown or you don't have enough mana or rage. It does not cast abilities for you, select targets, analyze combat performance, or replace the original game action.

Unlike the earlier [WarriorStartAttack](https://github.com/babunidev/WarriorStartAttack), this version has class-specific built-in ability lists, runtime controls and a **learn-the-next-ability** feature that saves additional start/stop behaviors by class.

## Install

Copy both committed files into the AddOns directory with this folder name:

    Interface/AddOns/MeleeStartAttack/
      MeleeStartAttack.toc
      MeleeStartAttack.lua

Restart or reload the game, enable **Melee Start Attack**, and look for the chat message *Melee Start Attack loaded.* A printed load message verifies Lua execution, **not** successful interception of every action API.

The TOC defines SavedVariables **MeleeStartAttackDB**; learned abilities survive reloads/logins, but the simple on/off toggle is **reset to enabled at every load**.

### Recommended attack-state setup

Put WoW's ordinary **Attack** action (from the General spellbook tab) on **any action bar**. The addon scans slots 1–120, remembers an Attack action slot and uses **IsCurrentAction** to check whether basic swings are on. If there is no Attack action present, it uses an in-memory flag updated through combat events and its own calls.

This fallback is less precise: being *in combat* isn't necessarily identical to actively auto-attacking. Repeated presses are **designed** to avoid toggling attacks off, but that behavior has not been verified on an actual client during this audit.

## Supported default ability families

| Class | Built-in start-attack actions |
| --- | --- |
| Paladin — **7 icons** | Seal of Righteousness, Seal of the Crusader, Seal of Light, Seal of Wisdom, Seal of Justice, Seal of Command, Judgement |
| Warrior — **20 icons** | Bloodthirst, Cleave, Demoralizing Shout, Disarm, Execute, Hamstring, Heroic Strike, Intercept, Mortal Strike, Mocking Blow, Overpower, Pummel, Rend, Revenge, Shield Bash, Slam, Sunder Armor, Taunt, Thunder Clap, Whirlwind |
| Shaman — **5 icons** | Earth Shock, Flame Shock, Frost Shock, Stormstrike, **Lightning Strike (custom-ability mapping)** |

The implementation uses **32 class-scoped icon mappings**, not unique spell identifiers. Some ability names are historical source/README labels; the game client must supply matching internal icon textures. Vanilla/custom-client icon collisions or changed textures can produce unexpected matches or omissions.

**Built-in stop-attack exception:** **Warrior Intimidating Shout**. Additional stop actions can be learned for the current class.

## Commands

The primary command aliases are **/meleeattack** and **/msa**:

| Command | Result |
| --- | --- |
| `/msa` or `/msa toggle` | Toggle automatic start/stop behavior |
| `/msa on` / `/msa off` | Enable or disable automatic triggering **for this session** |
| `/msa status` | Report whether enabled and which auto-attack detection mode is active |
| `/msa learn` or `/msa learn start` | Learn the next recognized ability for **starting** auto-attack |
| `/msa learn stop` | Learn the next recognized ability for **stopping** auto-attack |
| `/msa list` | List learned abilities for your current class |
| `/msa unlearn <spell name>` | Remove the first matching learned **localized spell name** for your class |
| `/msa reset` | Remove all **learned** entries for your current class; built-in icons remain |
| `/start_auto_attack` | Manually start swings if a living hostile/attackable target exists and attack seems off |
| `/stop_auto_attack` | Manually stop swings if attack seems active |

For example, type **/msa learn stop** and press a spell the addon can identify by an internal icon and spellbook name. It will save a **class-specific** entry with `icon`, localized `spellName` and action `stop`. The learned entry follows the ability's **icon** if you move it between action-bar slots. The same press that completes learning also passes through regular start/stop matching; it is not a read-only capture event.

The manual `/start_auto_attack` and `/stop_auto_attack` commands are independent of the automatic enable toggle. The current code also permits learning entries for a class beyond the three with default lists, because learned entries are keyed by whatever class token the client returns; **three classes** is the declared built-in coverage, not a hard runtime restriction for custom learning.

## How ability recognition works

1. Action bar: wrap **UseAction** and query **GetActionTexture**.
2. Spellbook: wrap **CastSpell** and query **GetSpellTexture**.
3. Macros and other addon commands: wrap **CastSpellByName**, find the locally named ability in the spellbook, then compare its **icon**.

These wrappers call the original function after checking the icon. The effect occurs **before** the client can reject a spell for low resources or cooldown. They do not guarantee that a spell succeeded; they do not deliberately auto-cast, queue an ability or change the target.

An icon can be whitelisted and learned at the same time. In the current source **stop takes precedence** over start when a texture is recognized in both lists; learning an opposite behavior does not consistently erase a built-in mapping.

## Persistence and version migrations

MeleeStartAttackDB stores **customAbilities** by **class**, not separate profiles for individual characters. Different characters of the same class using one account SavedVariables table may share learned entries, even if localized names or abilities differ. Earlier version **1.1.2** stored **customStartIcons**; the current code migrates these to records with action **start** and placeholder name **(legacy learned icon)**, then clears the legacy table. To give an old entry a usable name, use learning again on its ability.

No other preferences are saved in the source (in particular, enabled/disabled state and cached Attack-action slot are session-only).

## Known limitations and next checks

- The in-combat fallback does not prove that actual white swings are active, so toggling **AttackTarget()** can be wrong if state drifts.
- Learning is **by icon**, not unique spell ID. Shared icon artwork and same-class custom spells are ambiguous.
- Learning needs a recognizable spellbook name; some macro/custom buttons cannot be identified and may leave learning armed for the next press.
- `/msa off` does not disable the two **manual** start/stop commands.
- Stop/start logic is separate from spell success, mana, rage, GCD and cooldown rules.
- Global function wrappers can conflict with other addons wrapping the same functions, including the related WarriorStartAttack addon.
- True multilingual compatibility was not independently tested; it is the **icon-based design goal**.
- This repo has **no LICENSE file**. Do not assume an open-source license or add one without Babuni.dev's decision.

The [roadmap](ROADMAP.md) gives a reproducible target-client test matrix. Everything above describes current code, **not proven gameplay performance**.

## Portfolio and development record

The repository began **August 22, 2026** as a broader class-based alternative. On August 29 it added custom ability learning and direct cast/macro interception; on August 30 it improved attack-state detection; on August 31 it gained **per-class saved start and stop choices**, including migration from older data. September 18 made naming provider-neutral. October 26 brought Babuni.dev author credit and structured documentation.

- [Creative concept](CONCEPT.md) · [Design](docs/DESIGN.md) · [Ability inventory](docs/CONTENT.md)
- [Architecture](docs/ARCHITECTURE.md) · [Decisions](docs/DECISIONS.md) · [Narrative development journal](docs/DEVLOG.md)
- [Roadmap / verification](ROADMAP.md) · [Changelog](CHANGELOG.md)
- [Human–AI collaboration](docs/AI_COLLABORATION.md) · [Agent rules](AGENTS.md)

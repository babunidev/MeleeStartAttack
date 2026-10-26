# MeleeStartAttack — decisions, recorded iterations and open questions

**Human decision authority:** Babuni.dev. The record below uses source/diff evidence, not invented private conversation or implied approval.

## Implemented design path

| Subject | Evidence | State |
| --- | --- | --- |
| Extend beyond Warrior | Original Aug 22 `ef0acb0` tables, README | Paladin/Warrior/Shaman built-in coverage |
| Rely on icon paths, not translations | Original and current Lua comments | Implemented; texture collisions remain possible |
| Observe attempted use, not success | Wrappers before delegating `UseAction` and cast functions | Implemented; client execution unverified |
| Stop on Intimidating Shout | Warrior `stopAttackIcons` table | Built-in behavior |
| Learn unknown class abilities | Aug 29 `2eafab6` | Started as start-only icon collection |
| Broaden macro/spell entry points | Aug 29 `2eafab6` | Three global functions wrapped |
| Hybrid/cached Attack mode | Aug 30 `02515ea` | Real `IsCurrentAction` if Attack slot found, else local fallback |
| Learn **start or stop** and persist metadata | Aug 31 `83f9b37` | Saved class-grouped entries, commands, migration |
| Generic custom-client label | Sep 18 `11e9387` | Removed provider-specific comment; no compatibility test documented |

## Specific tradeoffs and ambiguities

The goal of avoiding **AttackTarget** toggle-off motivates a state check. The installed Attack action is a more direct clue than **PLAYER_ENTER_COMBAT**, but requires the user to place Attack on a bar. The event fallback lowers setup requirements but cannot guarantee actual swing state.

The original Aug 22 version only wrapped **UseAction**, expressly avoiding the other global cast functions. The Aug 29 implementation **added CastSpell and CastSpellByName** so macros or spellbook casts could trigger. This is an observable direction change, not proof of any particular discussion over compatibility.

Learning by icon allows easy handling of custom spells whose numeric IDs aren't available to Lua 5.0. It also makes different spells sharing artwork indistinguishable, and uses the *first* spellbook matching name/icon when learning. A learned opposite direction is not always an override: if the icon is both start and stop, **stop gets priority**.

The initial learning format saved icon booleans per class. Aug 31 changed the schema to records including action and localized name, while migrating older icons with a **legacy placeholder**. That migration itself is evidence of supporting existing user settings, but no user acceptance/performance result is recorded.

## Open decisions, not committed work

1. Should per-class sharing be changed to per-character configuration?
2. Should the enabled/disabled setting persist across reloads?
3. Should learned opposite-direction actions override built-in defaults rather than obey stop-first priority?
4. Should the project restrict custom learning to exactly three classes, or allow it elsewhere as its code currently does?
5. Should the fallback combat flag be replaced with a stricter requirement for an Attack action?
6. How should unreachable/unidentifiable macro buttons be handled while learning?
7. Should the missing LICENSE be resolved, and under what license chosen by Babuni.dev?
8. Which Vanilla/custom builds and localized clients are officially supported based on real test evidence?

The documentation pass did not implement or approve any of these alternatives.

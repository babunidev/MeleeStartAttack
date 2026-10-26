# MeleeStartAttack — verification plan

**Creator / decision authority:** Babuni.dev  
**Status:** Planned tests and open choices. **No new feature or code change approved in this documentation pass.**

## Verified as present in source

- Manifest version 1.3.1, interface **11200**, one Lua source file and account-wide SavedVariables table **MeleeStartAttackDB**.
- Class-specific built-in start lists: **7 Paladin, 20 Warrior, 5 Shaman** icon paths; **1 Warrior** built-in stop icon.
- Three global wrappers: **UseAction**, **CastSpell**, **CastSpellByName**.
- Attack-action slot cache + **IsCurrentAction** status, with combat-event boolean fallback.
- Slash enable/status/learn/list/unlearn/reset and manual start/stop commands.
- Per-class persistence with migration from 1.1.2's **customStartIcons** to **customAbilities**.
- No test framework or live in-game results in the inspected repo.

## Priority 0 — attack state, player intent and toggles

1. Install on the **intended actual Vanilla 1.12.1-compatible client**. Record game build, addon load, slash registration and errors.
2. Put normal **Attack** on a bar; use `/msa status` to verify detected slot, then press a supported ability at insufficient resources and while ability is on cooldown. Confirm that only the **auto-attack** starts, not that the ability succeeds.
3. Press again while already swinging: ensure `AttackTarget()` is not invoked to stop it.
4. Move or remove the Attack action, test cached-slot invalidation and observe the switch to **fallback mode**.
5. With no Attack action on any bar, test entering/leaving combat, switching targets, manually starting/stopping swings and other addons changing attack state; note where the combat boolean diverges from actual swinging.
6. Test no target, friendly/unattackable, dead target, hostile living target. Confirm the start branch obeys each gate.
7. Test the Warrior Intimidating Shout stop icon while attacking, and restarting swings afterwards.
8. Test manual `/start_auto_attack` and `/stop_auto_attack` while automation is disabled with `/msa off`; confirm the commands still work as coded.

## Priority 1 — learning, migration and input variants

9. Learn **start** and **stop** actions through a normal action button, spellbook and `/cast`/CastSpellByName macro separately. Record the actual learned spellbook name/icon and whether the learning press also acts.
10. Confirm learned behaviors follow an ability when moved to another action slot and remain after reload/login.
11. Test sharing of learned entries across characters with the **same class**, and separation between Paladin, Warrior and Shaman lists.
12. Test **1.1.2 saved-variable migration**: preserve start behavior, placeholder `(legacy learned icon)`, and clearing of old `customStartIcons` keys. Back up SavedVariables before manipulating them.
13. Test **`/msa list`**, **`unlearn <localized name>`**, **`reset`**, invalid commands and session-only on/off default.
14. Test an unrecognized macro action: learning may remain armed until a resolvable spellbook name/icon is provided.
15. Test collisions between built-in and learned start/stop on the same icon: the implementation checks **stop first** and the learned entry does not automatically override a built-in list.
16. Verify the specific **Lightning Strike custom icon mapping** on the actual server; do not assume it exists in standard Vanilla.
17. Test more than three classes when learning custom entries: current runtime stores by current class, even though official built-in coverage is for three.
18. Verify all **32** default start icon mappings, the one stop mapping and client-language compatibility with real localized game clients.
19. Test load-order composition with addons that also replace global spell/casting functions, especially **WarriorStartAttack**.

## Priority 2 — development and release quality

20. Decide how this repo should be **licensed**; there is no LICENSE committed.
21. Consider a test harness for deterministic icon/mode selection, while separately recording true client tests for WoW APIs.
22. Capture sanitized videos or screenshots demonstrating the behavior (including intended failures and limits) for the portfolio.
23. Update implementation docs, decision record, DEVLOG and changelog with every actual fix and commit.

## Pending creator decisions, not approvals

- Is the fallback combat flag adequate, or should reliable Attack-slot presence become a mandatory setup condition?
- Should learning show an optional confirmation before the capturing action also starts/stops attacks?
- Should learned start/stop choices override built-in mappings, or should built-in behavior remain dominant with stop-first priority?
- Is per-class sharing across every character of that class the intended user experience?
- Is automatic re-enabling on each login desirable?
- Should there ever be a GUI, or is a small slash-command workflow part of the project's appeal?

These options remain open until Babuni.dev explicitly selects a direction.

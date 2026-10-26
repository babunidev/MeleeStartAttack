# MeleeStartAttack — source architecture (v1.3.1)

**Sources:** `MeleeStartAttack.lua` (435 lines), `MeleeStartAttack.toc`, 2026-10-26. **Static code inspection**; no client runtime test.

## Flow through the three intercepted functions

```text
UseAction(actionSlot, checkCursor, onSelf)
    -> GetActionTexture(actionSlot)
CastSpell(spellSlot, bookType)
    -> GetSpellTexture(spellSlot, bookType)
CastSpellByName(spellName, onSelf)
    -> find matching localized spellbook name
    -> GetSpellTexture(matching spellSlot)
           |
           v
     if automation enabled:
        if learning mode armed:
           learn icon + spellbook name for current class
        if IsStopAttackIcon(icon): StopAutoAttack()
        else if IsStartAttackIcon(icon): StartAutoAttack()
           |
           v
     invoke original WoW function with original arguments
```

These are direct wrappers of **global game functions**, not secure post-hooks. Other addons can hook the same globals; behavior and original delegation depend on load order. The current code recognizes source changes **at attempt time**, before checks for rage, mana, cooldown, range or cast success.

## Recognized class data

The hard-coded `startAttackIcons` has **7 PALADIN**, **20 WARRIOR**, **5 SHAMAN** entries. The five Shaman entries include a client-specific **Lightning Strike** assumption, labeled *custom ability* in the source. The `stopAttackIcons` table maps Warrior **Intimidating Shout** only.

Lookup uses `PlayerClass()` (the English class token returned by `UnitClass`), and checks that class's texture table, so default entries aren't global. **GetLearnedAbility(texture)** checks the current class's SavedVariables first. Both `IsStartAttackIcon` and `IsStopAttackIcon` return true if either the built-in table or matching learned action says yes; the wrapped call gives **stop first** priority.

**Conflict caution:** learning `stop` on a built-in start icon may cause both predicates to be true and stop wins. Learning `start` on a built-in stop icon similarly cannot override the stop. A learned record updates by **matching icon**, not by unique spell name.

## Hybrid auto-attack state

```text
GetAttackActionSlot():
   cached slot still IsAttackAction? -> reuse
   else scan action slots 1..120 for first IsAttackAction
   cache slot or nil

IsAutoAttacking():
   attack slot present? -> IsCurrentAction(slot)
   otherwise         -> attackActive in-memory flag

StartAutoAttack():
   UnitExists(target)
   AND not UnitIsDead(target)
   AND UnitCanAttack(player, target)
   AND not IsAutoAttacking()
      -> AttackTarget() [TOGGLE], attackActive = true

StopAutoAttack():
   if IsAutoAttacking(): AttackTarget() [TOGGLE]
   attackActive = false
```

On **PLAYER_ENTER_COMBAT**, `attackActive=true`; on **PLAYER_LEAVE_COMBAT**, `attackActive=false`. There is **no server-confirmed swing state** in fallback mode. Combat membership and active white attacks are different concepts, so the fallback requires game testing, especially when the Attack button is missing and another addon interacts with the toggle.

The source describes the cached Attack action as the more reliable mode. Unlike the August 22 version, later code does **not repeatedly scan all slots when its remembered Attack slot is still valid**, though moving/removing the Attack action triggers a rescan.

## SavedVariables and learning

```text
MeleeStartAttackDB (TOC SavedVariables; account-wide)
└── customAbilities
    ├── PALADIN = [{ icon, spellName, action="start"|"stop" }, ...]
    ├── WARRIOR = [...]
    ├── SHAMAN = [...]
    └── other class token if user explicitly learns one
```

On load the Lua initializes missing tables, and if `customStartIcons` exists, iterates it and inserts **start-only** records with `spellName="(legacy learned icon)"`, then clears the old field. That is explicit migration from the version-1.1.2 schema. **No per-character profile** exists; storage is by class across the account SavedVariables file.

`learnAction` is session-local (nil, `start` or `stop`). Learning an icon requires either a supplied `GetSpellName` from spellbook/cast or a successful `FindSpellNameByIcon` scan. A macro action without a resolvable spellbook name may fail to learn and leave the capture active. The **capturing input also passes through normal start/stop matching**; learned behavior can take effect on that same press.

Unlearning locates the first case-insensitive match by **localized spellName**, not icon, so migrated placeholder-name records are awkward to remove individually until relearned. `reset` clears current-class learned entries only; it does not change built-in lists.

## Runtime commands and enabled state

`SLASH_MELEESTARTATTACK1` and `2` register **/meleeattack** and **/msa**. They handle on/off/toggle/status/learn/learn start/learn stop/list/unlearn/reset. The code additionally registers standalone **/start_auto_attack** and **/stop_auto_attack**.

`enabled` is initialized `true` every Lua execution, with **no persisted enabled key**. The manual start/stop slash commands call their functions directly even when disabled; disabling only bypasses the wrapped input logic, not the commands themselves. Automatic lookup is class-scoped; manual start can work on any class with a valid hostile target.

## Compatibility and package shape

The TOC declares **Interface 11200**, **Version 1.3.1**, **Author Babuni.dev**, **SavedVariables MeleeStartAttackDB**, loading just **MeleeStartAttack.lua**. No custom GUI, media, network communication or external service was found. The source uses Vanilla-style **global event** in a simple registered event frame and Lua 5.0 syntax.

No automated tests, client logs or LICENSE were found in the inspected tree. This document describes source rules, **not experimentally verified game behavior**.

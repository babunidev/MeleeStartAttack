# MeleeStartAttack — narrative development journal

**Project creator and creative director:** Babuni.dev  
**Retrospective compiled:** October 26, 2026 from current Lua/TOC/README and dated Git diffs. This is **not** a verbatim restoration of earlier chats; motivations without direct evidence are identified as interpretations.

## A small Warrior idea becomes a family of interactions

The goal behind MeleeStartAttack is a simple form of respect for player intention. Someone clicks an attack-oriented ability to engage an enemy. In Vanilla WoW that ability may not succeed: the character may lack rage or mana, or the cooldown may not have completed. Those failures shouldn't necessarily keep the character from making ordinary melee swings. An addon can fill that tiny gap without attempting to make the player's decisions for them.

The creator had already explored the problem in the separate **WarriorStartAttack** project, uploaded earlier in August 2026. **MeleeStartAttack** broadens the approach beyond a Warrior-only whitelist: Paladin Seals and Judgement, a Warrior suite of actions, and Shaman Shocks and Stormstrike all gain the same kind of possible start trigger. The sources establish this close conceptual kinship; they don't prove a particular explicit port or original private conversation between the two repos.

The emotional benefit is not a new interface panel or a spectacular feature. It is the feeling that ordinary game controls do what the player expects them to do: if you engage a hostile target, your character begins attacking; if you are already swinging, repeated ability presses shouldn't turn that off.

## August 22, 2026 — first committed multi-class version

The repository's creation and first implementation commit **`ef0acb0`** are dated **August 22, 2026**. The initial file was roughly 150 lines and declared addon version **1.1.0** for interface **11200**, targeting Vanilla 1.12.1 / Lua 5.0.

This first implementation was intentionally small. It defined per-class icon tables for **Paladin, Warrior and Shaman**, a Warrior stop exception for Intimidating Shout, and a single interception point—**UseAction**—for action-bar presses. It checked a living attackable target and attempted to avoid blindly toggling `AttackTarget()`, a function that can both start *and stop* normal swings.

There was also an on/off/status slash-command system. The enabled state was deliberately (or at least as-coded) reset to true on each reload rather than stored. At that point, there was **no runtime learning system**, no macro-cast wrappers, no account-wide saved custom ability list, and no caching of the Attack action location.

The README described multi-class support and action-on-attempt behavior; it did not demonstrate a real game-client success. The exact original brainstorming and who authored particular lines were not available in this review, so the concept remains credited to **Babuni.dev**, and the implementation is described by what Git actually contains.

## August 29 — listening beyond action buttons and teaching the addon

The commit **`2eafab6`**, dated **August 29**, carried the title *“Refine melee start attack triggers.”* It expanded the Shaman table with a **Lightning Strike** icon, initially annotated as a special ability on a named custom provider. Later wording would remove that provider name, but the **custom-client origin of this mapping** is worth preserving rather than presenting it as a guaranteed base Vanilla ability.

The more substantial change was in how the addon understood an action. The original code deliberately wrapped only `UseAction` to avoid interfering with other spell-cast APIs. The revision added wrappers for **CastSpell** and **CastSpellByName**, making the behavior available when an addon, macro or spellbook invoked those APIs directly. That widened compatibility ambitions, but also widened potential interactions with other addons that hook global functions.

That week also introduced **`/msa learn`**: the next recognized icon could become a new start-attack rule for the player's class, stored in **MeleeStartAttackDB.customStartIcons**. No longer did every new or custom class ability require a Lua edit. The player could teach the system from inside the game. It was still start-only learning and it saved raw icon keys rather than named records.

The source marks these mechanisms as features present in code. No in-game test log was recovered to prove which macros, localized spell names, or custom providers they worked with.

## August 30 — the problem with AttackTarget being a toggle

On **August 30**, commit **`02515ea`** made the state model more deliberate and versioned the TOC to **1.2.1**. The core challenge is that WoW's `AttackTarget()` isn't a one-way StartAttack function. It toggles. If the addon calls it on every eligible button press, the player can accidentally cancel swings.

Earlier code scanned action-bar slots looking for an active Attack action, then fell back to a boolean based on combat enter/leave events. The new version looked for the **normal Attack action**, cached a found slot, and queried the game's `IsCurrentAction` state directly when that slot remained valid. If no Attack action could be found, it retained the old combat-event boolean as a compromise for players who hadn't placed Attack on a bar.

This revision also introduced explicit **/start_auto_attack** and **/stop_auto_attack** macro commands. Together, these changes made state detection more visible and gave players controlled direct commands in addition to automatic action-trigger interception.

The compromise remained imperfect in theory: entering combat and auto-swinging are not synonymous. The addon might infer an attack from combat events when no white swings are running. That is an architectural risk, **not a confirmed regression**. The development record can explain it without inventing a debugging incident.

## August 31 — personalizing behavior by class

The next day's commit **`83f9b37`** advanced the addon to **version 1.3.1**, adding more refined **per-class learning**. Instead of storing only a set of icons that should start attacks, each learned record contained the icon path, the localized spellbook name and an action of **start** or **stop**. Players could type **/msa learn stop** and select an ability that should stop auto-attacking, inspect the list with **/msa list**, remove entries by localized name or reset that class's learned rules.

Importantly, this update **migrated** older 1.1.2 saved icon collections into named-entry data while retaining their original start behavior. The migrated names were marked as **(legacy learned icon)** and could be refreshed by relearning. That is a visible concern for continuity, even if we don't have a discussion showing who originally requested backward compatibility.

This revision also contains a subtle constraint: an icon is still the central identity, and the runtime checks **stop before start**. If a built-in start icon is also learned as stop, both match; the stop wins. Conversely, learning start on the built-in Intimidating Shout stop icon won't remove its stop behavior. The system is therefore a combination of static class decisions and user learning, not an unrestricted override model.

The new learning records are shared across characters **of the same class** within the account-wide SavedVariables table, not saved per character. That choice may be convenient or surprising depending on the creator's intended use; it should be tested and discussed rather than retroactively justified.

## September 18 — separating the addon from a particular provider name

Commit **`11e9387`** on **September 18** changed the Lightning Strike code comment from naming a particular custom WoW provider to the neutral phrase **“custom ability.”** The source functionality stayed the same in the diff. The project is described generally for Vanilla 1.12.1 now; that textual generalization is **not evidence** of testing every Vanilla-compatible client or that Lightning Strike exists on all of them.

This is a worthwhile portfolio detail because it makes a distinction between **code that was ported or described more broadly** and **code whose runtime compatibility has been demonstrated**.

## October 26 — documentation and attribution

On **October 26**, the author field was standardized to **Babuni.dev** in the TOC (`d224a17`) and README (`ffc449c`). The first concept, roadmap, architecture, decisions and development-journal scaffold was created (`b0c710a`), then linked from README (`27e8bfd`).

In this current session, an AI documentation collaborator inspected the full 435-line source and its Git diffs, compiled the built-in **7 + 20 + 5** icon inventory, identified the learned-action override/trust limits, and wrote a more substantial concept, architecture, user guide, decision log and narrative journal.

**Creator contribution:** authored product purpose, constraints and creative direction embodied in the project. **AI contribution here:** source interpretation, history synthesis, technical warnings and Markdown commits. **Not performed:** source edits, game-client execution, actual multilingual testing, new accepted features or a reconstruction of undocumented personal conversations.

## What belongs in the next journal entry

The next development milestone is not more speculative prose. It is careful **in-game verification** of the attack-state modes and learned actions: a living hostile target at low resources, the presence/absence of an Attack button, localized macros, shared icon artwork, persistent learned records, and coexistence with other hooked addons. When Babuni.dev tests these, record what happened—including failures, alternatives considered and explicit decisions made—and update the portfolio record to match.

# MeleeStartAttack — interaction design

**Creative author:** Babuni.dev.

## A tiny intervention at the moment of intent

The user already pressed Heroic Strike, Judgement or Shock to engage a hostile target. The addon should not demand a separate auto-attack activation before normal melee begins. Its interface is therefore deliberately almost invisible: it adds a **conditional action-state change** to the player's ordinary interaction with the game.

The most visible UI is chat feedback from **/msa**. There is no options window, performance graph, combat overlay or automated target selection. Built-in icon tables are complemented by a short in-game learning ritual that removes the need to modify code for each custom ability.

## Learning as a design gesture

Typing **/msa learn start** or **/msa learn stop** sets a one-use capture mode. The next eligible ability press resolves an icon and a local spellbook name, and writes a class-scoped rule. The same rule stays with the ability when moved to another action slot, because the icon remains the key.

A failed capture (e.g. an unrecognizable macro icon) may leave learning active. The input used to learn is also processed as a gameplay input rather than suppressed, so the user should experiment on a safe target. A future confirmation/replay preview might help, but none is implemented or approved here.

## Attack-action mode versus fallback

The README encourages placing the ordinary **Attack** action on any bar. The addon can display which mode is used through **/msa status**. It will prefer the real game action's active-state flag over its own estimate, making normal button presses more predictable. When absent, it falls back to an event-based flag that can be wrong about active swings.

This is a useful two-tier compromise between reliable control and no extra setup. Making that tradeoff transparent in the user guide helps someone diagnose a toggle problem without a hidden interface.

## A meaningful stop gesture

Warrior Intimidating Shout has an explicit stop behavior, and any class can learn custom stops. This expresses that **not every action signals fighting more intensely**; some represent disengagement or control. Stop behavior takes precedence when a texture belongs to both start and stop lists.

## Per-class, not per-character personalization

One account-wide SavedVariables table holds the learned entries grouped by class. That shares rules across same-class characters; it does not represent individual character preferences. The automatic enable toggle is intentionally (or at least currently) ephemeral and resets on reload.

The creator has not yet specified whether a per-character opt-in system or separate GUI is desired. These remain options, not decisions.

## Testable ergonomic concerns

- Does the tooltip/status make it clear that normal Attack-action mode is preferred?
- Is the name-based unlearn command predictable in a localized client?
- Should custom stop rules override built-in start behavior in a visible way?
- Are alerts adequate when learning remains armed after an unsupported spell?
- Is a session-only on/off toggle surprising after reload?

Only direct in-game tests with representative users can answer these. This documentation pass changed no UI or behavior.

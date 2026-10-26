# MeleeStartAttack — attribution and collaboration record

**Human creator and creative director:** Babuni.dev.

## What can be supported

The current repository and dated commits show a creative idea to connect player-selected melee/casting actions with normal auto-attacks in old WoW, broadening earlier Warrior-only experiments to Paladin, Warrior and Shaman. They show the implementation changing from a one-hook whitelist to a three-entry-point, cached-Attack-action, class-learnable start/stop system.

They do **not** by themselves establish which lines were originally produced by a human, which were authored with an AI, why every change was discussed or whether any proposal was approved outside recorded changes. Do not attribute the creator's initial idea to AI or fabricate missing conversation.

## AI assistance in this documentation session (2026-10-26)

An assistant:
- Fetched the live GitHub tree, full `MeleeStartAttack.lua` and TOC, existing README, earlier docs and dated August/September Git diffs.
- Identified the **7 Paladin, 20 Warrior, 5 Shaman** default icon entries and one Warrior stop icon.
- Explained the three global wrappers, icon-based spellbook localization, player-intent triggering, cached Attack slot vs combat-flag fallback, per-class persistence, legacy migration, and precise command behavior.
- Flagged unverified edge cases: shared-icon start/stop priority, capturing presses that also trigger actions, session-only on/off, same-class cross-character storage, macro learning failure and interaction with other global hooks.
- Rebuilt 11 Markdown docs including the narrative development history, roadmap and agent rules.

This is **editorial, research and technical analysis work**, not a claim that the assistant conceived the project, changed combat code or tested the client. The current documentation commit is **Markdown-only**.

## Method for future changes

Every substantial discussion/implementation should preserve:
1. **User direction:** What Babuni.dev wanted and why, including limits.
2. **AI interpretation/proposal:** Alternatives, tradeoffs, and what the assistant actually contributed.
3. **Creative iteration:** Misunderstandings, mistakes, rejected ideas and refinements.
4. **Creator decisions:** What was explicitly accepted, rejected or postponed.
5. **Implementation:** Changed paths, commit/PR reference and migration implications.
6. **Verification:** Test environment, commands, actual outcomes and what remains unverified.

Create publishable summaries rather than raw private transcripts. Keep credentials, unrelated private details, and other players' private communications out of the portfolio. Respect the creator's choice of a code license; there is **no LICENSE** committed as of this pass.

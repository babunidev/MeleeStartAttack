# MeleeStartAttack — class abilities and data vocabulary

**Project creative director:** Babuni.dev. **Source authority:** actual version 1.3.1 Lua tables, README and TOC.

## Built-in start mappings

These names come from the repo's public README and accompanying icon table. They are not translated at runtime; **internal icon paths are the actual keys**. The project does not bundle original game icons.

| Class | Ability | Internal texture suffix |
| --- | --- | --- |
| Paladin | Seal of Righteousness | `Spell_Holy_SealOfRighteousness` |
| Paladin | Seal of the Crusader | `Spell_Holy_HolySmite` |
| Paladin | Seal of Light | `Spell_Holy_HealingAura` |
| Paladin | Seal of Wisdom | `Spell_Holy_RighteousnessAura` |
| Paladin | Seal of Justice | `Spell_Holy_SealOfWrath` |
| Paladin | Seal of Command | `Ability_Warrior_InnerRage` |
| Paladin | Judgement | `Spell_Holy_RighteousFury` |
| Warrior | Bloodthirst | `Spell_Nature_BloodLust` |
| Warrior | Cleave | `Ability_Warrior_Cleave` |
| Warrior | Demoralizing Shout | `Ability_Warrior_WarCry` |
| Warrior | Disarm | `Ability_Warrior_Disarm` |
| Warrior | Execute | `INV_Sword_48` |
| Warrior | Hamstring | `Ability_ShockWave` |
| Warrior | Heroic Strike | `Ability_Rogue_Ambush` |
| Warrior | Intercept | `Ability_Rogue_Sprint` |
| Warrior | Mortal Strike | `Ability_Warrior_SavageBlow` |
| Warrior | Mocking Blow | `Ability_Warrior_PunishingBlow` |
| Warrior | Overpower | `Ability_MeleeDamage` |
| Warrior | Pummel | `INV_Gauntlets_04` |
| Warrior | Rend | `Ability_Gouge` |
| Warrior | Revenge | `Ability_Warrior_Revenge` |
| Warrior | Shield Bash | `Ability_Warrior_ShieldBash` |
| Warrior | Slam | `Ability_Warrior_DecisiveStrike` |
| Warrior | Sunder Armor | `Ability_Warrior_Sunder` |
| Warrior | Taunt | `Spell_Nature_Reincarnation` |
| Warrior | Thunder Clap | `Ability_ThunderClap` |
| Warrior | Whirlwind | `Ability_Whirlwind` |
| Shaman | Earth Shock | `Spell_Nature_EarthShock` |
| Shaman | Flame Shock | `Spell_Fire_FlameShock` |
| Shaman | Frost Shock | `Spell_Frost_FrostShock` |
| Shaman | Stormstrike | `Ability_Shaman_Stormstrike` |
| Shaman | Lightning Strike (custom ability) | `Ability_ThunderClap` |

**Totals:** 7 + 20 + 5 = **32 class-specific built-in start icon entries**. These are mapping entries, not guarantees of 32 distinct server-side abilities. The Lightning Strike note is explicitly **custom** and its actual availability/icon can differ by client. Do not market it as a standard Vanilla Shaman ability.

## Built-in stop

The single predefined stop entry is **Warrior — Intimidating Shout**, referencing **`Ability_GolemThunderClap`**.

Other behaviors can be learned as local **start** or **stop** icon assignments for the current class.

## Learned records and localization

Each persisted record has:
- **icon:** internal `Interface\\Icons\\...` path;
- **spellName:** current client's localized name from the spellbook;
- **action:** either `start` or `stop`.

The name supports listing and unlearning. The **icon** controls triggering and determines whether a new learned entry updates an old entry of the same class. Icon sharing can conflate spells and a translated name is not universal across client languages.

## Other text content

There are no authored story scenes, images, emotes or guild recruitment messages. Chat text reports on/off, mode, learning success/failure and learned lists. Game images are referenced but not redistributed.

Game-client API spell/icon behavior and all translations remain **unverified** in this documentation session. Never attribute built-in Blizzard art to Babuni.dev or the AI documentation assistant.

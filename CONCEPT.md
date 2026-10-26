# MeleeStartAttack — concept

**Creative author:** Babuni.dev  
**Implementation target:** Vanilla WoW 1.12.1 / Lua 5.0.  
**Status:** A source-grounded creative reconstruction, not the missing original planning dialogue.

## When the player chooses to fight

The practical problem is small but persistent: selecting an offensive ability and beginning an ordinary melee attack can be two separate steps in old WoW. In low-rage and low-mana moments, or with a spell unavailable on cooldown, the player still intends to engage. **MeleeStartAttack** treats the player's chosen action as the signal to begin normal swings when a living hostile target is present.

The addon doesn't automate decisions about which attack to cast. It reacts to what the user already pressed, then delegates to the game's original ability invocation. Keeping ordinary player intent intact is central to the concept.

This expands the separate WarriorStartAttack idea to **Paladin, Warrior and Shaman**. Its key creative difference is not simply more predefined abilities, but letting the player declare new start or stop actions *from within the game*.

## Making an old-client limitation into an interaction

Vanilla lacks the later spell-identity APIs common to modern addons. The implementation compares internal spell **icon paths**, which are largely independent of the client's translated spell names. This avoids maintaining a multi-language textual whitelist and can work for new custom abilities with familiar game icons.

Runtime learning turns that technical method into an approachable workflow: issue `/msa learn start` or `/msa learn stop`, then press the intended ability. The addon finds an icon and spellbook name, stores the behavior under the character's **class token**, and remembers it after reload. It follows a moved action-bar button because the icon, not the original slot number, is the identity.

The downside is that **icons are not unique spell IDs**; two unrelated spells can use the same art. Real correctness requires careful client tests, especially on custom server builds.

## The right rhythm for a toggling attack API

The API function `AttackTarget()` toggles attack state. Calling it blindly on every offensive press can stop the player's swings. The project therefore seeks the normal Attack action on an action bar and uses its current-state flag. If no Attack action is present, it falls back to a combat-event-driven local guess.

August development progressively focused on that problem: moving from repeated scanning/flag checks to caching a genuine Attack action location. The idea was to **keep the addon invisible** while preventing an invisible mistake from interrupting combat.

## A start-and-stop language

The built-in lists cover 7 Paladin, 20 Warrior and 5 Shaman actions. Warrior's **Intimidating Shout** is a built-in stop exception. Learning extends both sides: players can mark abilities as **start** or **stop** without editing Lua. The macro commands offer a direct manual option.

This is an embodied usability idea, not a damage rotation. There are no efficiency metrics, recommendations, graphics panels or target-selecting AI.

## What remains unknown

The August and September Git commits document the progression of code and readmes. They do not reveal all reasoning, debugging sessions or original human–AI conversations. The creator's authorship is primary; source inspection by an AI assistant later cannot retroactively assign individual concept decisions to itself.

The future portfolio should show the evolution from a single-class intervention to an extensible three-class toy for improving feel, with implementation tradeoffs and actual client-test results kept distinct.

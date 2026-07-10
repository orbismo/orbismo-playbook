# Tabletop RPG

A persistent, narrative-forward, single-player tabletop RPG with the assistant as Game Master. The game's mechanics and canon are not baked into the prompt; they live in the world as `rule` entities and `canon`-tagged entities, so the campaign survives across sessions. Dice rolls are shown in the open, NPCs remember how you treated them, quests unlock structurally, and the world keeps moving when you look away.

## What's inside

**`instructions.md`** defines how the GM operates: two modes (Build for world-building, Play for in-character sessions), authority and trust precedence, how canon works, and how the propagating systems interact.

**`world.json`** seeds the rules engine: twelve `rule` entities, each carrying its full spec as lore.

| Rule | Role |
| --- | --- |
| Core Resolution | d20 + attribute vs. difficulty, rolled in the open. |
| World Canon | Governance of setting truth. Owns the `canon` tag and holds the first-init onboarding that builds a blank world into a playable one. |
| Propagation Budget & Surfacing | The conductor. Caps how many consequences one action fires and forbids narrating arithmetic; the player feels fiction, not numbers. |
| Disposition & Standing | NPC attitude as live graph state (from -3 to +3) that feeds social rolls and ripples through faction and rivalry edges. |
| Quest & Consequence | Quests as prerequisite dependency graphs; consequences as causal chains the world remembers. |
| Turn & Scene Workflow | The per-turn loop, closing with a durable-state check. |
| Session Logging & Save | One live autosaved save event per campaign plus an immutable session-history chain. |
| Character Creation / Sheet & Progression | Attributes, Vitality, XP, leveling. |
| Death, Downing & Recovery | What happens at 0 Vitality, and the stakes ladder. |
| Inventory & Equipment | Gear and how it feeds the dice. |
| Narration Style | Second person, tight vivid prose, visible rolls. |

## Installation

1. Create a new Orbismo world.
2. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
3. Give `world.json` to your connected AI and ask it to add its entities and relationships to the world, keeping names exactly as given.
4. Say hello. On a fresh template there is no setting yet, so the GM enters Build mode and runs the first-init onboarding with you (genre, world, and character) before any play begins.

## Customizing

- **Mechanics**: every mechanic is a rule entity; edit its lore to house-rule the game. Priority and trust decide conflicts, so your self-authored changes outrank the shipped defaults.
- **Setting**: canon is whatever entities carry the `canon` tag. Build your own world in Build mode, or import one you've written elsewhere.
- **New systems**: additional propagating systems (rumor networks, faction territory) follow the same pattern as Disposition & Standing: state on edges, ripples bounded by the conductor.

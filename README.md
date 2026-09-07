# The Awakening

An All Out multiplayer game. The video-game expression of The Nexus Arcanum (WyldePhyre / Kyla).

Post-apocalyptic urban fantasy junk-punk. Magic-centric: the Weave is the engine of the world, not flavor. Magic is not in the current `.csl`. That is a gap, not a decision to stay mundane.

The loop is colony-builder first. Scavenge, build, raise an outpost, rebuild society after the Awakening. Combat and quests support that loop.

The tabletop chassis is a hack of Monte Cook Games’ Cypher System. Our setting and procedures. This repo does not contain Cypher rule text.

## Already in the code

- Types: Scrapper, Builder, Seeker, Weaver
- Pools: Muscle, Spark, Edge
- Systems: outpost, crafting, gathering, combat, quests
- Day-1 Corsair Cove logistics: cliff shelves + lumber → planks → warehouse (`docs/CORSAIR-PIVOT-SLICE1.md`)

The original PRDs were lost. This README is the recovered stub, not the rulebook.

## Run the lumber-chain demo

1. Open `ao.project` in All Out (project **The Awakening**, id `69abc6e36a2374002d7404f1`).
2. Play as host / local (or a client — production is not gated on `Game.is_server()`).
3. Create a character. Lake cold-open has **no** Cove HUD and does **not** auto-seed.
4. Editor: **Unlock Cove (debug)** (stand-in for Snyder arrival). Walk east of your position.
5. Goods move Canopy → Midcliff → Docks. HUD **Cove** shows stock. **Take Planks** / **Seed Demo** as needed.

# Corsair Cove pivot — Day-1 slice

Phase: logistics only. All Out / CSL chassis. No Unreal. No new engine.

Wiki path names stay **Notoriety / Empire / Seafaring / Wealth**.
Forge flavor paths stay **Renown / Bastion / Range / Salvage**.
Crown is the antagonist. Fleet cards → Weave expeditions are **out of scope** (Forge owns naming).

## Hypothesis check (before inventing)

Verified against `scripts/outpost/outpost.csl` and `scripts/gathering/`:

| Needed | Existing? | Reused? |
| --- | --- | --- |
| Cliff / shelf pads | No. Outpost was a flat circular `grid_x/grid_y` plot. | Extended the same `Placed_Structure` + plot. `shelf` is first-class; `grid_y` mirrors it for old saves. |
| Woodcutter / sawmill | No All Out building IDs. `WORKSHOP` is a crafting-cost bonus. | Added `WOODCUTTER` / `SAWMILL` kinds on the existing structure registry. |
| Warehouse | `STORAGE` expands **player inventory**, not a goods buffer. | Left `STORAGE` alone. Added `WAREHOUSE` for the chain. |
| Lumber / planks | No. Nodes drop salvage / food / metal / etc. | Added `LUMBER` and `PLANKS` on the existing `Resource_Type` registry. Did **not** add a parallel grove-node system; the woodcutter has an implicit canopy stand. |
| Fetchers | None. | Thin haul jobs (brown lumber, tan planks) between the three buildings. |

Muscle / Spark / Edge and Scrapper / Builder / Seeker / Weaver are untouched.

## What shipped

1. **Three cliff shelves** (3 slots × 3 terraces): Docks (0), Midcliff (1), Canopy (2). Build mode picks a shelf, then a slot.
2. **Production triangle** on those shelves:
   - Canopy `Woodcutter` produces lumber every 2s (cap 8).
   - Haul lumber down to Midcliff `Sawmill`.
   - Sawmill converts 1 lumber → 1 plank.
   - Haul planks down to Docks `Warehouse` (cap 24).
3. **Runnable demo**
   - New outposts seed the triangle automatically (east of spawn at `10, 0`).
   - HUD **Cove** panel shows shelf stock + in-transit counts.
   - **Seed Demo** re-places any missing chain building.
   - **Take Lumber / Take Planks** pulls warehouse stock into the existing inventory.
4. World draw: terrace pads, canopy trees (visual only), building stock labels, moving haul quads.

## How to run the demo

1. Open this repo in All Out. Project entry: `ao.project` → **The Awakening** (`69abc6e36a2374002d7404f1`).
2. Play as **host / local** (production ticks on the server).
3. Finish character creation (any type).
4. Walk east of spawn to the three stacked shelves.
5. Watch brown lumber leave the canopy, convert on midcliff, and tan planks stack in the dock warehouse (~5s to first stored plank).
6. HUD → **Cove** for the readout. **Seed Demo** if you demolished a building.

Manual place: HUD → **Build** → pick shelf (Docks / Midcliff / Canopy) → Woodcutter / Sawmill / Warehouse. Costs use existing salvage / scrap metal so the chain can be rebuilt from the current economy.

## What is next (not this PR)

- **Rope / ziplines / ladders** — hauls are currently a straight lerp down the cliff. Next slice should make vertical links first-class and paint green/red path health.
- **Powder** — second goods chain (fiber / charcoal / powder) on a later shelf. Do not explode the catalog to 50+ goods.
- **Weave expeditions** — fleet-card slot becomes expedition cards. Forge owns the names. Do not wire cards until that pass.
- Four path questlines, naval combat, pool/type renames, Unreal art.

## File map

- `scripts/outpost/outpost.csl` — shelves, three building defs, seed
- `scripts/outpost/production.csl` — tick + hauls + warehouse withdraw
- `scripts/outpost/cove_ui.csl` — Cove panel
- `scripts/outpost/build_ui.csl` — shelf picker
- `scripts/crafting/recipe_registry.csl` — `LUMBER` / `PLANKS`
- `scripts/player.csl`, `scripts/ui/hud.csl` — tick + Cove button

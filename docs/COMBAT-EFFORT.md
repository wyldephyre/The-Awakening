# Combat Slice 1 — Effort spends (All Out / CSL)

Realtime slash and dodge stay. This slice only charges Cypher pools when those abilities fire.

## Cypher map (locked)

| Cypher pool | Awakening pool | Combat spend |
| --- | --- | --- |
| Might | Muscle | Slash (`Melee_Attack`) |
| Speed | Edge | Dodge (`Dodge_Roll`) |
| Intellect | Spark | unused this slice |

Do not rename the pools. HUD already labels Muscle / Spark / Edge.

## Soft costs

Small integers, documented here and as constants on the ability classes (not a second cost table):

- `SLASH_MUSCLE_COST :: 1` in `scripts/combat/melee_attack.csl` (next to `SLASH_DAMAGE` / `SLASH_COOLDOWN`)
- `ROLL_EDGE_COST :: 1` in `scripts/combat/dodge_roll.csl` (next to `ROLL_COOLDOWN`)

If the pool is too low to pay, the ability does not activate (Cypher-honest). No slash lunge, no dodge roll, no cooldown start.

Spends are not gated on `Game.is_server()`. Ability `can_use` / `on_update` run with the existing All Out ability path. UI stays under `Player.ao_late_update` + `is_local_or_server()`.

## Save and regen (existing systems)

No new persistence. After a spend:

- HUD bars read `player.muscle` / `player.edge` and update on the next local draw.
- `save_player_data` already writes those fields (`ao_end` on the server).
- `ao_update` already regen-ticks each pool by +1 every `POOL_REGEN_INTERVAL` (10s) when below max.

Crafted Muscle Stim / Edge Booster still restore the same fields.

Out of scope: Weave Encounter Tells/Responses, school tricks, Cove, path systems, pool renames, full Cypher difficulty ratings.

## How to playtest (host / local)

All Out does not compile or run in the Linux cloud VM. Use the Windows editor / local twin.

1. Open `ao.project` in All Out (project id `69abc6e36a2374002d7404f1`).
2. Play as **host / local**.
3. Finish character creation. Confirm HUD shows Muscle / Spark / Edge (base 10 plus type/descriptor).
4. **Slash:** click-aim Attack. Muscle should drop by 1. Slash lunge still happens.
5. **Dodge:** use the Dodge button / keybind and aim. Edge should drop by 1. I-frames roll still happens.
6. **Fail if broke:** slash until Muscle is 0. Attack must not fire (button disabled / no lunge). Dodge until Edge is 0. Dodge must not fire.
7. Wait ~10s: Muscle and Edge should each tick +1 if below max. Slash/dodge work again after the pool can pay.
8. Leave and rejoin: saved Muscle/Edge should match what you left with (regen during the session included).

If PR4 (`hotbar_default` / multi-return / `rng_root_seed`) is not yet on `main`, this branch includes those compile fixes so the project builds.

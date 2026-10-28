# UCP Regions for Radical Cobblemon Trainers

A datapack for [Ultimate Cobblemon Progression](https://github.com/Lucalaure/ultimate-cobblemon-progression) (UCP) region mode. It splits every Cobblemon species into regions and turns Kanto into a region driven by gym battles:

- A **Kanto series** for [Radical Cobblemon Trainers](https://modrinth.com/mod/rctmod) (RCT) with the 8 gym leaders, the Elite Four and the Champion, using FireRed/LeafGreen-style teams.
- The trainers live in the gyms from [Radical Gyms & Structures](https://modrinth.com/datapack/radical-gyms-cobblemon) (RGS). Their natural spawning is turned off.
- A **UCP Kanto region** with 10 tiers: you start with Kanto's weakest species, each gym badge unlocks the next tier, and beating the Champion unlocks the rest and completes the region.

## Requirements

- Cobblemon 1.8, Ultimate Cobblemon Progression 1.4+
- Radical Cobblemon Trainers 0.19+ (and its RCT API library)
- Radical Gyms & Structures 0.7+ (and CobbleFurnies). Command blocks must be enabled on servers.

## Setup

1. Put this pack in the world's `datapacks/` folder, either as a folder or zipped with `pack.mcmeta` at the top of the zip. You can also load it into every world with a global datapack mod such as [Open Loader](https://modrinth.com/mod/open-loader). Gyms only generate in new terrain, so a new world is best.
2. Check `/datapack list`: this pack must come **after** `rgs`, so its trainer settings win.
3. Start the series: talk to a Trainer Association NPC, or run `/rctmod player set series kanto`.
4. Find gyms with `/locate structure rgs:pewter_gym` (also `cerulean`, `vermilion`, `celadon`, `fuchsia`, `saffron`, `cinnabar` in the Nether, `blackthorn`) and `rgs:kanto_league` in the End.

## Progression

| Order | Trainer | Strongest Pokémon | Unlocks Kanto tier |
|---|---|---|---|
| 1 | Leader Brock | 14 | 2 |
| 2 | Leader Misty | 21 | 3 |
| 3 | Leader Lt. Surge | 24 | 4 |
| 4 | Leader Erika | 29 | 5 |
| 5 | Leader Koga | 43 | 6 |
| 6 | Leader Sabrina | 43 | 7 |
| 7 | Leader Blaine | 47 | 8 |
| 8 | Leader Clair | 50 | 9 |
| 9–12 | Elite Four Lorelei, Bruno, Agatha, Lance | 54–60 | — |
| 13 | Champion Blue | 63 | 10, and completes Kanto |

RCT's level cap is the strongest Pokémon of your next required trainer (never below 15), so it runs 15, 21, 24, 29, 43, 43, 47, 50, 54, 56, 58, 60, 63, then 100 after the Champion.

## How it works

- **Trainers** (`data/rctmod/`) reuse RCT's own trainer ids, because those are what RGS's gyms summon. Redefining an id replaces RCT's version of that trainer: our teams, our `kanto` series, and our gym order. RCT's built-in skins and badge advancements still apply.
- **The region** (`data/ultimate-cobblemon-progression/regions/kanto/`) lists which species are in each tier and which RCT advancement unlocks it. The tier files are generated from Cobblemon's regional Pokédexes by [`scripts/generate-region-tiers.ps1`](https://github.com/Lucalaure/ultimate-cobblemon-progression/blob/main/scripts/generate-region-tiers.ps1) in the UCP repo. To regenerate a region, run it from a UCP checkout with `-OutDir` pointing at this pack's `data/ultimate-cobblemon-progression/regions/<region>`.

## The other regions

Every other region is included as a **starting choice**: each player can open one of them from the start with `/trainerlevel region choose <region>`:

| Region | Species | Region | Species |
|---|---|---|---|
| Johto | 100 | Kalos | 72 |
| Hoenn | 135 | Alola | 86 |
| Sinnoh (with Hisui's 7) | 114 | Galar | 89 |
| Unova | 156 | Paldea | 120 |

None of them have trainers yet, so their 10 tiers unlock with the **same Kanto badges** as Kanto's tiers: the chosen region grows alongside Kanto. Regional forms (Alolan Vulpix, Galarian Ponyta, …) belong to their base species' region, so they're part of Kanto, Johto and so on.

They have no `completedBy`, so each counts as complete once its last tier is unlocked (the Kanto Champion). In practice, beating the Kanto Champion currently opens every region fully. When a region gets its own trainers, swap its tier requirements for their advancements and give it a `completedBy`.

Meltan and Melmetal aren't in any regional Pokédex, so they have no region (and don't spawn naturally in Cobblemon anyway).

## Adding the next region

Add a `regions/<region>/` folder with `"order": 2` and its own tier files. It opens automatically once Kanto's `completedBy` advancement (the Champion) is earned. Its trainers need their own RCT series that requires Kanto (`"requiredSeries": [["kanto"]]`).

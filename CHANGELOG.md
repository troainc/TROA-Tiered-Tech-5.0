# Changelog

## [5.0.15] — 2026-09-22

### Fixed

- **Cargo containers, lockers, armory lockers, and weapon racks still showed ~9.2
  trillion L and did not tier.** The 5.0.14 `TROA5_EntityComponents.sbc` was
  generated with the polymorphic type written as a plain `type="…"` instead of
  `xsi:type="…"` (a namespace-prefix bug in the generator), so Space Engineers did
  not recognize the entries as `InventoryComponentDefinition`s and silently
  dropped them — leaving inventory volume unbounded exactly as before. The
  generator now preserves the `xsi:type` attribute; all 294 tier inventory
  components load, so volumes are correct and scale per tier (Large Cargo
  Container: 3x ≈ 1.27M L → 18x ≈ 7.59M L). Re-publish the Workshop item to apply.

## [5.0.14] — 2026-09-21

### Fixed

- **Cargo containers, lockers, armory lockers, and weapon racks showed ~9.2
  trillion L (unlimited) and did not scale per tier.** Space Engineers defines an
  inventory block's storage volume with an `InventoryComponentDefinition` in
  `EntityComponents.sbc` (keyed by subtype), and the 5.0 generator never created
  those for the tier subtypes — so the 5.0.9 inventory containers had no size and
  defaulted to unbounded. Added `TROA5_EntityComponents.sbc` (294 entries) with
  the vanilla inventory size **scaled by tier**, so volumes are correct and tier
  up (e.g. Large Cargo Container: vanilla 421,875 L → 3x ≈ 1.27M L → 18x ≈ 7.59M
  L), matching how gas-tank capacity already scales.

## [5.0.13] — 2026-09-21

### Fixed

- **More "Block-pair … is not in the same block-variant group" server errors.**
  Some blocks (corner LCDs, corner LCD flats, 2-corner interior lights,
  transponders) have no `BlockPairName`, so 5.0.8's BlockPairName grouping put the
  large and small counterparts in separate groups even though Space Engineers
  pairs them by grid size. The group builder now normalizes the `Large`/`Small`
  grid prefix for these blocks so both counterparts share one group, and the
  validator checks grid counterparts too. Verified: 0 pair-rule violations.

## [5.0.12] — 2026-09-21

### Fixed

- **LCD / text panels had no settings in the control panel.** Space Engineers
  attaches the LCD surface component through a wildcard entity container
  (`TextPanel` / `SubtypeId="*"` → `MyObjectBuilder_LcdSurfaceComponent`), and
  that wildcard does not reach mod-added tier subtypes — so every tiered LCD,
  text panel, corner LCD, billboard, and lab screen came up with no surface and
  no control-panel options. The container generator now materializes wildcard
  containers explicitly for tier blocks, so tiered LCDs get their surface
  component (and settings) like the vanilla panels.

## [5.0.11] — 2026-09-20

### Removed

- **Tier copies of blocks driven by another mod's code, which broke when tiered.**
  Placing tiered **AI blocks** (Flight Movement, Path Recorder, Offensive/Defensive
  Combat, Event Controller) crashed the client/server, and tiered **weapons**
  (interior turret, large/small gatling + missile turrets/launchers) disappeared
  under WeaponCore — because that code only recognizes the original subtypes.
  Removed all 192 of those tier definitions and every reference to them (build
  categories, variant groups, entity containers). Players use the original
  vanilla/WeaponCore blocks for these instead; all other tiered blocks are
  unchanged. Warhead, decoy, and safe-zone tiers are kept.
- Note: the Defense Shields "heat" spam is **not** a TROA block — TROA does not
  tier any shield block, so that is the Defense Shields mod/server config, not
  Tiered Tech.

## [5.0.10] — 2026-09-20

### Fixed

- **Upgrade modules didn't scale with tier.** Tiered Productivity / Effectiveness
  (yield) / Energy modules kept vanilla `Modifier` values at every tier, so a 18x
  module did nothing more than a 3x one. Modifiers now scale by tier — additive
  modifiers by the tier number (Productivity 0.5 → 9.0 at 18x), multiplicative
  modifiers by scaling the bonus (Effectiveness 1.09 → 2.63, Energy 1.22 → 5.01 at
  18x).
- **Oxygen/Hydrogen generators didn't scale.** `IceConsumptionPerSecond` now scales
  by tier, so higher-tier generators process ice and produce gas that many times
  faster.

### Added

- `docs/UPGRADE_MODULE_SCALING.md` — a player-facing table of every module type and
  generator throughput per tier, with worked examples.

## [5.0.9] — 2026-09-20

### Fixed

- **Tiered inventory blocks showed a maximum of 0 L and crates/lockers would not
  open.** Modern Space Engineers attaches a block's inventory through
  `EntityContainers.sbc` (a per-subtype map to an `MyObjectBuilder_Inventory`
  component), and the 5.0 generator copied the block definitions but not those
  container entries — so every tiered cargo container, small/large/industrial
  container, bulk container, cargo terminal, locker, armory locker, weapon rack,
  and conveyor-access block had no inventory component. Generated
  `TROA5_EntityContainers.sbc` with container entries for all tiered blocks
  (948 entries), so inventory volume and open/close now work exactly like the
  vanilla base blocks. Modded cargo blocks (e.g. AQD conveyor access) get a
  generic inventory container fallback.

## [5.0.8] — 2026-09-20

### Fixed

- **Thousands of server "Block-pair … is not in the same block-variant group"
  errors.** The tier scroll-groups added in 5.0.5 were keyed by block subtype,
  which put the large block (e.g. `Window1x2Inv9x`) and its small-grid partner
  (`SmallWindow1x2Inv9x`) into different groups even though they share a
  BlockPairName — and Space Engineers requires both halves of a block-pair to be
  in the same variant group, logging an error per pair otherwise (~4,000+ on the
  full server). Groups are now keyed by **BlockPairName** (tier stripped), so the
  large block, its small-grid partner, and all six tiers live in one group, and
  every block is included so no pair-half is orphaned. Verified: 0 block-pair
  violations, 0 blocks in more than one group. The per-family
  `TROA5_VariantGroups_*.sbc` files are replaced by a single
  `TROA5_VariantGroups.sbc` (1,021 groups covering all 9,216 tiered blocks). The
  in-game "+" tier scrolling is unchanged.

## [5.0.7] — 2026-09-19

### Fixed

- **Unsupported `.DDS` texture paths (SE 01_210_014).** The generator copied
  vanilla/DLC definitions verbatim, and 11 vanilla icon/reflector references
  carried an uppercase `.DDS` extension that the current engine rejects
  (`File extension of ... .DDS is not supported`). Across six tiers this produced
  ~2,100 `mod_warning` entries per mod copy. The generator now normalizes texture
  extension case to lowercase `.dds`, which resolves to the identical game asset.
- **Obsolete wheel mount-point warnings.** Generated `Wheel` (rotor-part)
  definitions inherited an empty `<MountPoints></MountPoints>` from the vanilla
  catalog. SE resolved that to `null`, fell back to the deprecated auto-generated
  default, and logged `Obsolete default definition of mount points` (~192 per mod
  copy). The generator now writes explicit full-face mount points that reproduce
  the engine default exactly, so build/attach behavior is unchanged.
- **Stale `Components2x` reference.** Regeneration dropped a dangling
  `<Class>Components2x</Class>` production-class reference that no longer exists in
  source (the previous generated output predated its removal).

### Validation

- `tools/validate_troa5.py` now flags any uppercase `.DDS` reference and verifies
  every generated `Icon`/`Model`/`ReflectorTexture` path resolves to a real file
  in the mod or the game content. `tools/generate_troa5.py` can now run without
  Pillow via `--skip-artwork` (auto-enabled when Pillow is absent).

_No SubtypeIds, gameplay values, tiers, components, or menu structure changed._

## [5.0.6] — 2026-09-19

### Changed

- The tier scroll-group icons now carry a custom TROA **tier-group badge**
  overlay (gold chevron stack + "TIER" frame) on top of the block icon, matching
  the branded look of the rest of the mod instead of showing a plain vanilla
  block icon. Uses the same two-icon compositing the tiered blocks already use.

## [5.0.5] — 2026-09-19

### Added

- Tier scroll-groups. Every tiered block shape is now a single build-menu entry
  with a **+** badge; mouse-wheel scrolls it through its tiers
  (3x → 6x → 9x → 12x → 15x → 18x), the same way vanilla armor scrolls through
  cube/slope/corner. 1,497 groups cover 8,982 blocks, collapsing the menu from
  roughly nine thousand flat entries to about fifteen hundred scrollable icons so
  blocks are far easier to find. Light and heavy stay as separate groups and the
  existing family subcategories are unchanged.

## [5.0.4] — 2026-09-19

### Fixed

- The mod loaded only 4 of 6 phases with a null-reference error on the bundled
  Contact/Fieldwork cargo containers. The bundled Contact, Fieldwork, Apex
  Survival Pack, and Economy 2 blocks still required the old 2x/4x/8x/16x Tech
  components that were removed in 5.0.2, so their build recipes could no longer
  resolve. Those recipes now use the current Tech components (3x/6x/9x/18x), and
  a malformed mount-point line on the modular cargo containers was corrected.
  The full mod loads all six phases again.

## [5.0.3] — 2026-09-19

### Removed

- Custom respawn ships. The mod no longer overrides the respawn-ship list, so
  Space Engineers uses its default spawn ships. Removed `Data/Respawn/`
  (RespawnShips definition and the Odin spawn pod/rover prefabs).

## [5.0.2] — 2026-09-19

### Removed

- All legacy 2x / 4x / 8x / 16x tiers have been removed (the old TROA 4.0 tier
  system, including their Tech2x/4x/8x/16x components, recipes, and menu/economy
  entries). Only the active 3x–18x tiers remain. This trims the mod's definition
  count substantially and improves load stability alongside large mod sets.
- **Note:** grids still using old 2x/4x/8x/16x blocks will lose those blocks on
  load. Rebuild them with the current 3x–18x tiers.

## [5.0.1] — 2026-09-19

### Fixed

- Some tiered blocks — including tier cockpits and consoles — were not loading,
  which on dedicated servers could disconnect you when you tried to place one.
  All six tiers now load correctly in single-player and multiplayer. Block build
  costs are unchanged.

## [5.0] — 2026-09-19

### Added

- Enhanced 3x, Proficient 6x, Elite 9x, Legendary 12x, Asgardian 15x, and Odin
  18x progression.
- 9,216 tiered block variants across 1,536 buildable base blocks.
- Tier-scaled light/heavy armor protection across 232 armor shapes, panels, and
  eligible variants.
- Fixed 3x protection for all tiered non-armor functional blocks.
- One expandable TROA G-menu heading with 15 block-family subcategories plus
  Character Tools and Character Weapons.
- Tiered handheld tools and weapons with matching Tech-component upgrade recipes
  and tier-scaled effectiveness.
- Tier badge system, focus icons, tier cards, armor chart, and progression chart.
- Asgardian and Odin technology components and recipes with chained progression.

### Changed

- Active block names now begin with a searchable tier label such as
  `[15x Asgardian]`.
- Reworked the TROA menu into a single expandable DLC-style category instead of
  nine separate top-level entries.
- Expanded coverage to decorative blocks, structures, windows, mechanical
  systems, Prototech, and every other current build-menu family.
- Reduced tier-badge opacity so the underlying block and item artwork remains
  visible.
- Economy 2 compatibility is retained.

### Compatibility

- Legacy Tech2x, Tech4x, Tech8x, and Tech16x definitions remain loadable for
  existing grids.
- Legacy blocks and recipes are hidden from normal build and production
  categories.

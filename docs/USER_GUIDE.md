# TROA Tiered Tech 5.0 — player and server guide

Tiered Tech is a Space Engineers Workshop mod that adds tiered equipment and progression. The GitHub repository documents the mod; subscribe to the official [Workshop item](https://steamcommunity.com/sharedfiles/filedetails/?id=3804068289) to install it. It is not a Torch command plugin, and there is no separate server configuration file for the tier catalog.

## Install and first use

Subscribe to the Workshop item, add it to the world/server mod list, and wait for definitions to load. Check the server log for definition/XML errors before testing. On a test world, open the G menu and search for Tiered Tech blocks or select the intended block category. Build the block and inspect its component requirements, inventory capacity, power use, and terminal controls. Clients and dedicated server must load a compatible mod version.

## Tiers and progression

The active progression includes 3x Enhanced, 6x Proficient, 9x Elite, 12x Legendary, 15x Asgardian, and 18x Odin. Higher tiers use Tech components to unlock craft progression. Check [TIERS.md](TIERS.md) for exact block/tier availability and recipes; the [upgrade module and generator scaling guide](UPGRADE_MODULE_SCALING.md) describes per-tier balance. Different block families do not all scale the same way: armor, tools, production, power, cargo/inventory, and utility functions have their own documented rules. Do not assume a displayed tier multiplier applies to every stat.

The G-menu category is organized by block family and subcategory. Search filters help locate a block; the tier name and subtype identify its performance class. Tiered handheld tools and weapons, where present in the current release, use the relevant recipe and Tech-component progression. DLC-dependent blocks still require the corresponding player entitlement.

## LCD and inventory behavior

Use vanilla LCD/text-panel blocks for displays. Standalone Tiered LCD/text-panel blocks have been removed as documented for v5.0.19 onward. Inventory volumes are scaled by tier but should remain finite; consult the current tier table and update/version notes if the displayed volume is unexpected. Check the grid's power availability for high-output generators and equipment.

## Troubleshooting and compatibility

After an update, allow normal server definition loading to finish. Confirm both server and clients have the same Workshop version. If a block is missing from the G menu, check category/filter, mod load errors, version, and DLC entitlement. If an inventory or control panel looks wrong, reproduce it on a fresh test grid and include the block subtype, tier, and log excerpt. Compatibility notes are in [COMPATIBILITY.md](COMPATIBILITY.md); the [README](../README.md) and [CHANGELOG](../CHANGELOG.md) give release boundaries. Static validation is not a substitute for in-game placement, balance, or multiplayer testing.

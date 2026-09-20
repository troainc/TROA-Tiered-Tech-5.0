# Tier Reference

Six active tiers. Each tier is unlocked by researching/holding the previous
tier's Tech component.

| Tech component | Tier name | Armor protection | Functional protection |
|----------------|-----------|-----------------:|----------------------:|
| `Tech3x`  | Enhanced   | 3x  | 3x |
| `Tech6x`  | Proficient | 6x  | 3x |
| `Tech9x`  | Elite      | 9x  | 3x |
| `Tech12x` | Legendary  | 12x | 3x |
| `Tech15x` | Asgardian  | 15x | 3x |
| `Tech18x` | Odin       | 18x | 3x |

## Protection

- **Armor** divides the block's damage taken by its tier (an 18x armor block
  takes 1/18 of the damage), preserving the light/heavy distinction.
- **Functional blocks** (decorative, structural, windows, utility, production,
  etc.) take fixed **3x** protection at every tier.

## Output & function

Functional stats scale by tier — production speed, power output and storage,
cargo capacity, jump range, thruster force, sensor and antenna range, gas
generator throughput, and upgrade-module strength. See
[UPGRADE_MODULE_SCALING.md](UPGRADE_MODULE_SCALING.md) for the exact per-tier
module and generator numbers.

## Cost

Higher tiers cost more to build, adding a matching **Tech** component to the
block's recipe (Tech3x → Tech18x), each requiring the previous tier's component
to produce.

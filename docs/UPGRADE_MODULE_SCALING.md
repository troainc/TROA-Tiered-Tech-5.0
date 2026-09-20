# TROA Tiered Tech 5.0 — Upgrade Module & Generator Scaling

How the tiered upgrade modules and gas generators scale. Every value scales by the
**tier number** (3x, 6x, 9x, 12x, 15x, 18x). Vanilla is shown as the 1x baseline.

## Upgrade modules

Each module type applies its effect **per module** attached to a block. Space
Engineers stacks them: additive modules add up, multiplicative modules multiply
together, up to the block's module-slot count.

### Productivity Module — production **speed** (additive)

Each module adds this much crafting/refining speed. `Modifier` is the value in the
mod; the bonus is per module.

| Tier | Modifier | Speed bonus per module |
|-----:|---------:|-----------------------:|
| Vanilla (1x) | 0.5 | +50% |
| 3x  | 1.5 | +150% |
| 6x  | 3.0 | +300% |
| 9x  | 4.5 | +450% |
| 12x | 6.0 | +600% |
| 15x | 7.5 | +750% |
| 18x | 9.0 | +900% |

Example: a refinery with **four** 18x Productivity modules runs at
1 + 4 × 9.0 = **37× speed**.

### Effectiveness Module — **yield** / material efficiency (multiplicative)

Each module multiplies how much you get out of the same ore. Modules **compound**
(two modules = Modifier × Modifier).

| Tier | Modifier (× per module) | Yield bonus per module |
|-----:|------------------------:|-----------------------:|
| Vanilla (1x) | 1.09 | +9% |
| 3x  | 1.27 | +27% |
| 6x  | 1.54 | +54% |
| 9x  | 1.81 | +81% |
| 12x | 2.09 | +109% |
| 15x | 2.36 | +136% |
| 18x | 2.63 | +163% |

Example: a refinery with **two** 18x Effectiveness modules yields
2.63 × 2.63 ≈ **6.9× materials**.

### Energy Module — **power efficiency** (multiplicative)

Each module multiplies the block's power efficiency (less power for the same work).
Modules compound.

| Tier | Modifier (× per module) |
|-----:|------------------------:|
| Vanilla (1x) | 1.22 |
| 3x  | 1.67 |
| 6x  | 2.34 |
| 9x  | 3.01 |
| 12x | 3.67 |
| 15x | 4.34 |
| 18x | 5.01 |

## Oxygen / Hydrogen generators

Higher-tier generators process ice faster, producing proportionally more gas.
`IceConsumptionPerSecond` scales by the tier number, so gas output per second scales
the same way (a tier scales throughput by that multiple over vanilla).

| Tier | Throughput vs vanilla |
|-----:|----------------------:|
| 3x  | 3× |
| 6x  | 6× |
| 9x  | 9× |
| 12x | 12× |
| 15x | 15× |
| 18x | 18× |

Example: a large O2/H2 generator that eats 25 ice/s at vanilla eats 450 ice/s at
18x and makes 18× the gas per second.

---

*Refinery/assembler base speed, thruster force, reactor/battery power, jump range,
and other block stats already scale by tier the same way. Cargo capacity, armor
protection, and build cost scale as documented in the main changelog.*

# Ironing Calibration

Per-color ironing settings tuned on Bambu Lab P2S. Values empirical — adjust per surface result.

Ironing is a **process** setting, not a filament one: `ironing_flow`, `ironing_speed`,
`ironing_type` and `ironing_spacing` are single scalars per plate, so they cannot be
stored per color in a filament profile. Per-color ironing is applied as a **per-object
override** in the project's `model_settings.config` (Bambu Studio: right-click object →
object settings). Record the values here and in each profile's `description` field.

Only **flow** and **speed** below are calibrated. Spacing, inset and angle are Bambu
system defaults, listed for reference — inset is `0.11` on a 0.2 nozzle and `0.21`
otherwise.

## SUNLU PETG (0.2 nozzle)

| Color | Flow | Speed | Notes |
|-------|------|-------|-------|
| White | 25% | 20 mm/s | spacing 0.15mm, inset 0.11mm, angle 45° — brass nozzle, stale |
| Black | 27% | 10 mm/s | spacing 0.15mm, inset 0.11mm, angle 45° — brass nozzle, stale |

White and Black predate the switch to the stainless steel 0.2 nozzle, which moved temps
to 260 °C and roughly doubled pressure advance. Both need remeasuring.

## SUNLU PETG (0.4 nozzle)

| Color | Flow | Speed | Notes |
|-------|------|-------|-------|
| Black | 35% | 10 mm/s | spacing 0.15mm, inset 0.21mm, angle 45° — 0.20 mm layer, remeasured 2026-10-25 after flow 0.94525 / PA 0.042 |

## Overture PETG (0.4 nozzle)

| Color | Flow | Speed | Notes |
|-------|------|-------|-------|
| Gold | 20% | 20 mm/s | spacing 0.15mm, inset 0.21mm, angle 45° |

## Test plates

Calibration grids live in `~/Descargas` as `.3mf`, laid out as velocity × flow with each
square an object carrying its own override (`30-10` = 30 mm/s, 10% flow):

| Plate | Filament |
|-------|----------|
| `test-initial-calibration-02(3).3mf` | PETG - Skin Beige 0.2 |
| `test-initial-calibration-02(4).3mf` | PETG - Pink 0.2 |
| `Ironing_Test_Complete_PETG.3mf` | PETG - White 0.2 |
| `Cube_B.3mf` | PETG - Yellow (0.4) |

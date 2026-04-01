# Bambu Lab P2S - Sunlu Filament Profiles

> A love letter to the 3D printing community and Sunlu users everywhere.

[Leer en Espanol](README_ES.md)

Custom-calibrated filament profiles for the **Bambu Lab P2S** printer with **Sunlu** PETG, PETG HS Matte, PLA, and PLA Wood filaments. Built on top of [Jamie Jones Jr's GEN2 profiles](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles) with **per-color calibration** for temperature, flow ratio, and pressure advance.

We share these freely because we believe the maker community grows stronger when knowledge is open. If these profiles save you a failed print or a wasted evening of troubleshooting, then this project has served its purpose.

---

## The Problem

Jamie Jones Jr's GEN2 community profiles are an incredible resource --- 503+ configurations covering dozens of Sunlu filaments across multiple Bambu Lab printers. His work is the foundation that made ours possible.

However, every color of filament --- even from the same brand and material --- behaves differently. Pigments and additives alter viscosity, flow characteristics, and thermal behavior. A single generic profile can't account for this. **You must calibrate each color individually.**

This project takes Jamie's excellent GEN2 base and adds per-color calibration on top.

---

## Profiles

### PETG (Sunlu Basic)

All PETG profiles are built on Jamie's GEN2 PETG BASIC base with these shared settings:
- **Exhaust fan:** 70% with M142 dynamic gcode control
- **Part cooling fan:** 30-60% (min-max)
- **Overhang fan:** 90% at 95% threshold
- **Max volumetric speed:** 16 mm3/s
- **Pre-cooling temperature:** 190C (for clean AMS cutter cuts)
- **Cut retraction distance:** 20mm
- **Z-hop:** 0.2mm with Spiral Lift

| Color | Temp (C) | Flow Ratio | Pressure Advance | Hex |
|-------|----------|------------|-----------------|-----|
| Black | 245 | 1.010 | 0.029 | `#000000` |
| White | 240 | 0.989 | 0.032 | `#FFFFFF` |
| Red | 245 | 0.989 | 0.052 | `#FF0000` |
| Blue | 250 | 0.97 | 0.046 | `#0000FF` |
| Yellow | 250 | 0.989 | 0.035 | `#FFD700` |
| Olive Green | 235 | 0.938 | 0.046 | `#708238` |
| Purple | 245 | 0.967 | 0.031 | `#800080` |
| Pink | 245 | 1.010 | 0.032 | `#FF69B4` |
| Skin Beige | 240 | 0.979 | 0.030 | `#D2B48C` |
| Gray | 245 | 0.979 | 0.045 | `#808080` |

**PETG Ironing settings (process profile):**

| Setting | Value |
|---------|-------|
| Pattern | Rectilinear |
| Speed | 30 mm/s |
| Flow | 50% |
| Line spacing | 0.15 mm |
| Inset | 0.21 mm |
| Angle | 45 degrees |

### PETG HS Matte (Sunlu High Speed)

| Color | Temp (C) | Flow Ratio | Pressure Advance | Hex |
|-------|----------|------------|-----------------|-----|
| Mint Green | 235 | 0.9984 | 0.032 | `#98FF98` |

HS Matte profiles use **0% exhaust** (different thermal behavior), fan 25-50%, hot plate 80/85C.

### PLA (Sunlu)

| Color | Temp (C) | Flow Ratio | Hex |
|-------|----------|------------|-----|
| Marble | 220 | 0.966 | `#E8E0D0` |

PLA profiles use **pre-cooling 170C** and cut retraction 20mm.

### Base Templates

Templates for calibrating new colors:
- `PETG - Base.json` --- Starting point for new PETG colors
- `PETG HS Matte - Base.json` --- Starting point for HS Matte colors
- `PLA - Base.json` --- Starting point for PLA colors
- `PLA Wood - Base.json` --- Starting point for PLA Wood colors

---

## How to Use

### Importing Profiles

1. Open **Bambu Studio**
2. Go to the filament settings
3. Click **Import** and select the `.json` profile file
4. The profile will appear with the format `PETG - [Color]` for easy identification

### Calibrating a New Color

Follow this workflow in order (each step depends on the previous one being stable):

#### Step 1: Temperature Tower

Print a temperature tower to find the optimal nozzle temperature. In Bambu Studio, go to the calibration menu and select **Temperature Tower** (enable Developer Mode in preferences if you don't see it).

**What to look for:**
- **Stringing** = temperature too high (filament too fluid)
- **Sagging bridges** = temperature too high (not enough viscosity)
- **Delamination / weak layers** = temperature too cold (poor bonding)
- **Rough surface** = temperature too cold (poor flow)

Pick the **lowest temperature** that gives clean bridges and strong layer adhesion.

#### Step 2: Flow Ratio

We use the [Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3) model. Print it and measure the single-wall line widths with calipers. Flow ratio = expected width / measured width.

**Why per-color:** Pigments change melt viscosity and die swell. Our PETG colors range from 0.938 (Olive Green) to 1.010 (Black, Pink) --- a 7% spread within the same brand and material.

#### Step 3: Pressure Advance (PA)

Use the built-in **Pressure Advance** calibration in Bambu Studio. It prints lines at different K factor values. Look for the value where corners are sharp without bulging or gaps.

Our PETG values range from 0.029 (Black) to 0.052 (Red).

#### Step 4: Retraction (if needed)

If you see stringing after PA calibration, fine-tune retraction length. Most of our PETG colors work well at the default values.

---

## The Science Behind the Settings

### Why the P2S is Different

The P2S is a fully enclosed printer. This creates challenges for PETG:

1. **Heat accumulates** in the chamber during long prints
2. **The exhaust fan** vents this heat, but also cools the hotend zone
3. **PETG has low MFR** (5.3 g/10min) --- if the hotend zone cools even slightly, viscosity spikes and the extruder stalls

### Dynamic Exhaust Control (M142)

Jamie's GEN2 profiles solve the exhaust problem with custom gcode in `filament_start_gcode`:

```gcode
M142 P6 R30 S40 U0.3 V0.8 ; PETG exhaust control
```

This uses the M142 command for **dynamic exhaust management** rather than a static fan speed, allowing the printer to adapt exhaust intensity based on chamber conditions. This is a major improvement over our earlier approach of capping exhaust at 35%.

### Pre-cooling Temperature: Clean Cuts in Multicolor

When the AMS changes filament, the nozzle cools before cutting:
- **Too hot** (> 210C): Filament still soft, messy cut
- **Too cold** (< 180C): Filament too rigid, motor overload on reload
- **Our value:** 190C for PETG, 170C for PLA

> **Important:** The `filament_pre_cooling_temperature_nc` (non-cutter) value must stay at 215C for PETG. Lowering it to 190C causes motor overload.

### Cut Retraction Distance

Before cutting, the filament retracts 20mm to pull the molten tip back and create a clean, cuttable section. Too short = cut on soft filament. Too long = air pulled into melt zone.

---

## Project Structure

```
SUNLU/
  0.4/
    PETG/
      PETG - Black.json
      PETG - White.json
      PETG - Red.json
      PETG - Blue.json
      PETG - Yellow.json
      PETG - Olive Green.json
      PETG - Purple.json
      PETG - Pink.json
      PETG - Skin Beige.json
      PETG - Gray.json
      PETG - Base.json
      PETG HS Matte - Mint Green.json
      PETG HS Matte - Base.json
    PLA/
      PLA - Base.json
      PLA Wood - Base.json
      PLA Marble.json
```

---

## Acknowledgments

- **[Jamie Jones Jr](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles)** --- Creator of the Sunlu GEN2 community profiles (503+ configurations). His GEN2 PETG BASIC profile is the structural base for all our PETG profiles. Thank you, Jamie.

- **[Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3)** --- The MakerWorld model we used for precise flow calibration of every color.

- **The Sunlu Profiles Discord community** --- For sharing knowledge and troubleshooting together.

- **Sunlu** --- For making quality filaments accessible to everyone.

---

## Contributing

Found a better setting? Calibrated a color we don't have?

1. Fork this repo
2. Calibrate your color following the workflow above (temperature -> flow -> PA)
3. Submit a PR with your calibrated profile and values in the description

---

## License

This project is licensed under the **GNU General Public License v3.0** --- see [LICENSE](LICENSE) for details.

You are free to use, modify, and share these profiles. Any derivative work must also be shared under the same license. **These profiles may not be sold.**

---

*Made with patience, many failed prints, and a lot of love for the maker community.*

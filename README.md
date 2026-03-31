# Bambu Lab P2S - Sunlu Filament Profiles

> A love letter to the 3D printing community and Sunlu users everywhere.

Custom-calibrated filament profiles for the **Bambu Lab P2S** printer with **Sunlu** PETG, PETG HS Matte, PLA, and PLA Wood filaments. These profiles were born from weeks of systematic calibration, testing, and learning --- and from the frustration of extruder jams that the community profiles caused on the P2S.

We share these freely because we believe the maker community grows stronger when knowledge is open. If these profiles save you a failed print or a wasted evening of troubleshooting, then this project has served its purpose.

---

## The Problem

The [Sunlu GEN2 community profiles](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles) by **Jamie Jones Jr** are an incredible resource --- 503+ configurations covering dozens of Sunlu filaments across multiple Bambu Lab printers. His work is the foundation that made ours possible, and we owe him a massive thank you.

However, on the **P2S specifically**, the enclosed chamber design creates a thermal environment that differs from the A1 and other open-frame printers. The GEN2 profiles use an exhaust fan speed of 70%, which on the P2S cools the hotend zone too aggressively. Combined with PETG's low Melt Flow Rate (5.3 g/10min at 230C), this causes the filament to solidify prematurely in the transition zone, leading to **extruder motor overload and jams**.

This project fixes that issue and takes it further: **per-color calibration** for temperature, flow ratio, and pressure advance.

---

## Profiles

### PETG (Sunlu Basic)

All PETG profiles share these global settings:
- **Exhaust fan:** 35% (safe maximum for P2S enclosed chamber)
- **Part cooling fan:** 10-50% (min-max)
- **Pre-cooling temperature:** 190C (for clean AMS cutter cuts)
- **Cut retraction distance:** 20mm
- **Z-hop:** 0.2mm with Spiral Lift

| Color | Temp (C) | Flow Ratio | Pressure Advance | Hex |
|-------|----------|------------|-----------------|-----|
| Negro | 245 | 1.010 | 0.029 | `#000000` |
| Blanco | 240 | 0.989 | 0.032 | `#FFFFFF` |
| Rojo | 245 | 0.989 | 0.052 | `#FF0000` |
| Azul | 250 | 0.97 | 0.046 | `#0000FF` |
| Amarillo | 250 | 0.989 | 0.035 | `#FFD700` |
| Verde Oliva | 235 | 0.938 | 0.046 | `#708238` |
| Morado | 245 | 0.967 | 0.031 | `#800080` |
| Rosa | 245 | 1.010 | 0.032 | `#FF69B4` |
| Beige Piel | 240 | 0.979 | 0.030 | `#D2B48C` |
| Gris | 245 | 0.979 | 0.045 | `#808080` |

### PETG HS Matte (Sunlu High Speed)

| Color | Temp (C) | Flow Ratio | Pressure Advance | Hex |
|-------|----------|------------|-----------------|-----|
| Mint Green | 235 | 0.9984 | 0.032 | `#98FF98` |

HS Matte profiles use **0% exhaust** (different thermal behavior), fan 25-50%, hot plate 80/85C.

### PLA (Sunlu)

| Color | Temp (C) | Flow Ratio | Hex |
|-------|----------|------------|-----|
| Marmol | 220 | 0.966 | `#E8E0D0` |

PLA profiles use **pre-cooling 170C** and cut retraction 20mm.

### Base Templates

Templates for calibrating new colors are included:
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

Every color of filament --- even from the same brand and material --- behaves differently. Pigments and additives alter viscosity, flow characteristics, and thermal behavior. **You must calibrate each color individually.**

Follow this workflow in order (each step depends on the previous one being stable):

#### Step 1: Temperature Tower

**What:** Print a temperature tower to find the optimal nozzle temperature for your specific color.

**How:** In Bambu Studio, go to the calibration menu and select **Temperature Tower**. If you don't see it, enable **Developer Mode** in preferences first. The tower prints at decreasing temperatures from top to bottom.

**What to look for:**
- **Stringing** between sections = temperature too high (filament too fluid, oozes during travel)
- **Sagging bridges** = temperature too high (not enough viscosity to hold shape)
- **Delamination / weak layers** = temperature too cold (poor inter-layer bonding)
- **Rough surface** = temperature too cold (poor flow)

Pick the **lowest temperature** that still gives clean bridges and strong layer adhesion.

**Why it matters:** A 5C change can shift flow ratio by 2-4% in PETG. Temperature must be locked in first because every subsequent calibration depends on it.

#### Step 2: Flow Ratio

**What:** Calibrate how much filament the extruder actually pushes vs what the slicer expects.

**How:** We use the [Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3) model from MakerWorld. Print it and measure the single-wall line widths with calipers. The model generates a correction factor.

**Why per-color:** Pigments change melt viscosity and die swell behavior. Darker pigments (carbon black) increase viscosity; lighter/translucent formulations flow more easily. Our PETG colors range from 0.938 (Verde Oliva) to 1.010 (Negro, Rosa) --- a 7% spread within the same brand and material.

**Theory:** Flow ratio = expected width / measured width. Values below 1.0 mean the printer is over-extruding; above 1.0 means under-extruding. Typical PETG range: 0.92-1.01.

#### Step 3: Pressure Advance (PA)

**What:** Compensate for the pressure buildup inside the melt zone between the drive gear and nozzle tip.

**How:** In Bambu Studio, use the built-in **Pressure Advance** calibration (in the calibration menu). It prints a pattern of lines at different K factor values. Look for the value where corners are sharp without bulging or gaps.

**What PA does:** Without compensation, pressure lags behind commanded flow:
- Before corners: pressure is still high when the head decelerates, causing **bulging**
- After corners: pressure is too low when the head accelerates, causing **gaps**

PA slightly retracts filament proportional to deceleration (before corners) and advances it during acceleration (after corners).

**The K factor** defines how aggressively this compensation acts. Our PETG values range from 0.029 (Negro) to 0.052 (Rojo). Too high = under-extrusion at corners; too low = corner bulges.

#### Step 4: Retraction (if needed)

If you see stringing after PA calibration, fine-tune retraction length. Most of our PETG colors work well at the default 0.7mm/0.4mm (Standard/High Flow).

---

## The Science Behind the Settings

### Why the P2S is Different

The P2S is a fully enclosed printer. This is great for temperature-sensitive materials (less warping, more consistent layers), but it creates challenges for PETG:

1. **Heat accumulates** in the chamber during long prints
2. **The exhaust fan** (called "Escape" in Bambu's app) is meant to vent this heat, but it also cools the hotend zone
3. **PETG has low MFR** (Melt Flow Rate: 5.3 g/10min) --- it resists flow. If the hotend zone cools even slightly, viscosity spikes and the extruder motor stalls

### Exhaust Fan: The Balancing Act

| Exhaust % | Effect on P2S |
|-----------|---------------|
| 0-20% | Safe, minimal chamber cooling. May get too hot on long prints |
| **35%** | **Sweet spot.** Enough ventilation without cooling the hotend |
| 50-70% | Risk zone. May cause intermittent jams depending on ambient temperature |
| 100% | **Will cause motor overload with PETG.** Confirmed through testing |

> **Pro tip:** If you want more cooling (like simulating an open door), increase the exhaust fan **manually from the Bambu app during printing** rather than setting it in the profile. This lets you find the limit for your specific environment without baking a dangerous value into the profile.

### Part Cooling Fan: Quality vs Adhesion

The part cooling fan blows directly on the freshly deposited layer:
- **Too little** (< 25%): Poor surface quality, sagging overhangs, the layer stays soft too long
- **Too much** (> 70%): Rapid cooling impairs inter-layer bonding, risk of delamination
- **Our setting** (10-50%): Balanced compromise for PETG

**Key finding:** Printing with the P2S door open dramatically improves surface quality. The ambient cooling is more uniform than fan cooling. This confirmed that part cooling was the main quality bottleneck, not the exhaust fan.

### Pre-cooling Temperature: Clean Cuts in Multicolor

When the AMS changes filament, the nozzle cools down before the cutter engages to make the filament rigid enough for a clean cut:
- **Too hot** (> 210C): Filament is still soft/stringy, messy cut, deformed tip that jams on reload
- **Too cold** (< 180C): Filament becomes too rigid, motor can't push the solidified plug afterward
- **Our value** (190C for PETG, 170C for PLA): Clean cut, reliable reload

> **Important:** The `filament_pre_cooling_temperature_nc` (non-cutter) value must stay at 215C for PETG. Lowering it to 190C causes motor overload. Only the cutter version (`filament_pre_cooling_temperature`) should be 190C.

### Cut Retraction Distance

Before cutting, the filament retracts 20mm to pull the molten tip back and create a thin, cuttable section at the blade location. Too short = cut on soft filament = deformed tip. Too long = air pulled into melt zone = voids on next extrusion.

---

## Project Structure

```
P2S (Studio Version 2.5.0.66)/
  SUNLU - P2S Fix/
    0.4 nozzle/
      PETG - Negro.json
      PETG - Blanco.json
      PETG - Rojo.json
      PETG - Azul.json
      PETG - Amarillo.json
      PETG - Verde Oliva.json
      PETG - Morado.json
      PETG - Rosa.json
      PETG - Beige Piel.json
      PETG - Gris.json
      PETG - Base.json
      PETG HS Matte - Mint Green.json
      PETG HS Matte - Base.json
      PLA - Base.json
      PLA Wood - Base.json
      PLA Marmol.json
```

---

## Acknowledgments

This project would not exist without:

- **[Jamie Jones Jr](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles)** --- Creator of the Sunlu GEN2 community profiles (503+ configurations). His work is the foundation we built upon. He is currently rebuilding all profiles from scratch for even better results. Thank you, Jamie.

- **[Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3)** --- The MakerWorld model we used for precise flow calibration of every color.

- **The Sunlu Profiles Discord community** --- For sharing knowledge, troubleshooting together, and pushing the boundaries of what these affordable filaments can do.

- **Sunlu** --- For making quality filaments accessible to everyone.

---

## Contributing

Found a better setting? Calibrated a color we don't have? We'd love your contribution:

1. Fork this repo
2. Calibrate your color following the workflow above (temperature -> flow -> PA)
3. Submit a PR with your calibrated profile

Please include your calibration values and any observations in the PR description.

---

## License

This project is licensed under the **GNU General Public License v3.0** --- see [LICENSE](LICENSE) for details.

You are free to use, modify, and share these profiles. Any derivative work must also be shared under the same license. **These profiles may not be sold.**

---

*Made with patience, many failed prints, and a lot of love for the maker community.*

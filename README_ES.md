# Bambu Lab P2S - Perfiles de Filamento Sunlu

> Una carta de amor a la comunidad de impresion 3D y a los usuarios de Sunlu en todas partes.

[Read in English](README.md)

Perfiles de filamento calibrados individualmente para la impresora **Bambu Lab P2S** con filamentos **Sunlu** PETG, PETG HS Matte, PLA y PLA Wood. Construidos sobre los [perfiles GEN2 de Jamie Jones Jr](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles) con **calibracion por color** de temperatura, ratio de flujo y pressure advance.

Compartimos estos perfiles libremente porque creemos que la comunidad maker crece cuando el conocimiento es abierto. Si estos perfiles te ahorran una impresion fallida o una noche de frustracion, este proyecto habra cumplido su proposito.

---

## El Problema

Los perfiles GEN2 de Jamie Jones Jr son un recurso increible --- mas de 503 configuraciones cubriendo docenas de filamentos Sunlu en multiples impresoras Bambu Lab. Su trabajo es la base sobre la que construimos.

Sin embargo, cada color de filamento --- incluso de la misma marca y material --- se comporta diferente. Los pigmentos y aditivos alteran la viscosidad, las caracteristicas de flujo y el comportamiento termico. Un perfil generico no puede tener esto en cuenta. **Debes calibrar cada color individualmente.**

Este proyecto toma la excelente base GEN2 de Jamie y agrega calibracion por color.

---

## Referencia de Nombres (Ingles - Espanol)

Los nombres de los perfiles estan en ingles. Esta tabla te ayuda a identificar cada color:

| Nombre del Perfil | Color en Espanol |
|---|---|
| PETG - Black | Negro |
| PETG - White | Blanco |
| PETG - Red | Rojo |
| PETG - Blue | Azul |
| PETG - Yellow | Amarillo |
| PETG - Olive Green | Verde Oliva |
| PETG - Purple | Morado |
| PETG - Pink | Rosa |
| PETG - Skin Beige | Beige Piel |
| PETG - Gray | Gris |
| PETG - Base | Base (plantilla) |
| PETG HS Matte - Mint Green | Verde Menta |
| PETG HS Matte - Base | Base HS Matte (plantilla) |
| PLA - Base | Base PLA (plantilla) |
| PLA Wood - Base | Base PLA Madera (plantilla) |
| PLA Marble | PLA Marmol |

---

## Perfiles

### PETG (Sunlu Basic)

Todos los perfiles PETG estan construidos sobre la base GEN2 PETG BASIC de Jamie con estos ajustes compartidos:
- **Ventilador de escape:** 70% con control dinamico M142 por gcode
- **Ventilador de pieza:** 30-60% (min-max)
- **Ventilador de voladizos:** 90% al 95% de umbral
- **Velocidad volumetrica maxima:** 16 mm3/s
- **Temperatura de pre-enfriamiento:** 190C (para cortes limpios del AMS)
- **Distancia de retraccion de corte:** 20mm
- **Z-hop:** 0.2mm con Spiral Lift

| Color | Temp (C) | Ratio de Flujo | Pressure Advance | Hex |
|-------|----------|----------------|-----------------|-----|
| Black (Negro) | 245 | 1.010 | 0.029 | `#000000` |
| White (Blanco) | 240 | 0.989 | 0.032 | `#FFFFFF` |
| Red (Rojo) | 245 | 0.989 | 0.052 | `#FF0000` |
| Blue (Azul) | 250 | 0.97 | 0.046 | `#0000FF` |
| Yellow (Amarillo) | 250 | 0.989 | 0.035 | `#FFD700` |
| Olive Green (Verde Oliva) | 235 | 0.938 | 0.046 | `#708238` |
| Purple (Morado) | 245 | 0.967 | 0.031 | `#800080` |
| Pink (Rosa) | 245 | 1.010 | 0.032 | `#FF69B4` |
| Skin Beige (Beige Piel) | 240 | 0.979 | 0.030 | `#D2B48C` |
| Gray (Gris) | 245 | 0.979 | 0.045 | `#808080` |

**Ajustes de planchado para PETG (perfil de proceso):**

| Ajuste | Valor |
|--------|-------|
| Patron | Rectilineo |
| Velocidad | 30 mm/s |
| Flujo | 50% |
| Espacio entre lineas | 0.15 mm |
| Insercion | 0.21 mm |
| Direccion | 45 grados |

### PETG HS Matte (Sunlu Alta Velocidad)

| Color | Temp (C) | Ratio de Flujo | Pressure Advance | Hex |
|-------|----------|----------------|-----------------|-----|
| Mint Green (Verde Menta) | 235 | 0.9984 | 0.032 | `#98FF98` |

Los perfiles HS Matte usan **0% escape** (comportamiento termico diferente), ventilador 25-50%, cama caliente 80/85C.

### PLA (Sunlu)

| Color | Temp (C) | Ratio de Flujo | Hex |
|-------|----------|----------------|-----|
| Marble (Marmol) | 220 | 0.966 | `#E8E0D0` |

Los perfiles PLA usan **pre-enfriamiento 170C** y retraccion de corte 20mm.

### Plantillas Base

Plantillas para calibrar nuevos colores:
- `PETG - Base.json` --- Punto de partida para nuevos colores PETG
- `PETG HS Matte - Base.json` --- Punto de partida para colores HS Matte
- `PLA - Base.json` --- Punto de partida para colores PLA
- `PLA Wood - Base.json` --- Punto de partida para colores PLA Wood

---

## Como Usar

### Importar Perfiles

1. Abre **Bambu Studio**
2. Ve a los ajustes de filamento
3. Haz clic en **Import** y selecciona el archivo `.json`
4. El perfil aparecera con el formato `PETG - [Color]`

### Calibrar un Nuevo Color

Sigue este flujo de trabajo en orden (cada paso depende del anterior):

#### Paso 1: Torre de Temperatura

Imprime una torre de temperatura para encontrar la temperatura optima. En Bambu Studio, ve al menu de calibracion y selecciona **Temperature Tower** (activa el Modo Desarrollador en preferencias si no lo ves).

**Que buscar:**
- **Hilos (stringing)** = temperatura muy alta (filamento muy fluido)
- **Puentes caidos** = temperatura muy alta (poca viscosidad)
- **Delaminacion / capas debiles** = temperatura muy baja (mala adhesion)
- **Superficie rugosa** = temperatura muy baja (mal flujo)

Elige la **temperatura mas baja** que de puentes limpios y buena adhesion entre capas.

#### Paso 2: Ratio de Flujo

Usamos el modelo [Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3). Imprimelo y mide el ancho de las paredes con un calibrador. Ratio de flujo = ancho esperado / ancho medido.

**Por que por color:** Los pigmentos cambian la viscosidad y el die swell. Nuestros PETG van de 0.938 (Olive Green) a 1.010 (Black, Pink) --- un 7% de diferencia dentro de la misma marca y material.

#### Paso 3: Pressure Advance (PA)

Usa la calibracion de **Pressure Advance** integrada en Bambu Studio. Imprime lineas a diferentes valores de factor K. Busca el valor donde las esquinas son limpias sin abultamientos ni huecos.

Nuestros valores PETG van de 0.029 (Black) a 0.052 (Red).

#### Paso 4: Retraccion (si es necesario)

Si ves hilos (stringing) despues de calibrar PA, ajusta la longitud de retraccion.

---

## La Ciencia Detras de los Ajustes

### Por que el P2S es Diferente

El P2S es una impresora completamente cerrada. Esto crea desafios para PETG:

1. **El calor se acumula** en la camara durante impresiones largas
2. **El ventilador de escape** saca este calor, pero tambien enfria la zona del hotend
3. **El PETG tiene bajo MFR** (5.3 g/10min) --- si la zona del hotend se enfria un poco, la viscosidad sube y el motor del extrusor se traba

### Control Dinamico de Escape (M142)

Los perfiles GEN2 de Jamie resuelven el problema del escape con gcode personalizado:

```gcode
M142 P6 R30 S40 U0.3 V0.8 ; control de escape PETG
```

Esto usa el comando M142 para **manejo dinamico del escape** en vez de una velocidad estatica, permitiendo que la impresora adapte la intensidad del escape segun las condiciones de la camara.

### Temperatura de Pre-enfriamiento: Cortes Limpios en Multicolor

Cuando el AMS cambia filamento, la boquilla se enfria antes de cortar:
- **Muy caliente** (> 210C): Filamento blando, corte sucio
- **Muy frio** (< 180C): Filamento muy rigido, sobrecarga del motor al recargar
- **Nuestro valor:** 190C para PETG, 170C para PLA

> **Importante:** El valor `filament_pre_cooling_temperature_nc` (sin cortador) debe quedarse en 215C para PETG. Bajarlo a 190C causa sobrecarga del motor.

---

## Estructura del Proyecto

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

## Agradecimientos

- **[Jamie Jones Jr](https://makerworld.com/en/models/2435491-jones-sunlu-lightbox-p2s-series-print-profiles)** --- Creador de los perfiles comunitarios Sunlu GEN2 (503+ configuraciones). Su perfil GEN2 PETG BASIC es la base estructural de todos nuestros perfiles PETG. Gracias, Jamie.

- **[Improved Flow Ratio Calibration V3](https://makerworld.com/en/models/189543-improved-flow-ratio-calibration-v3)** --- El modelo de MakerWorld que usamos para calibrar el flujo de cada color.

- **La comunidad Discord de Sunlu Profiles** --- Por compartir conocimiento y resolver problemas juntos.

- **Sunlu** --- Por hacer filamentos de calidad accesibles para todos.

---

## Contribuir

Encontraste un mejor ajuste? Calibraste un color que no tenemos?

1. Haz fork de este repo
2. Calibra tu color siguiendo el flujo de trabajo (temperatura -> flujo -> PA)
3. Envia un PR con tu perfil calibrado y los valores en la descripcion

---

## Licencia

Este proyecto esta licenciado bajo **GNU General Public License v3.0** --- ver [LICENSE](LICENSE) para detalles.

Eres libre de usar, modificar y compartir estos perfiles. Cualquier trabajo derivado debe compartirse bajo la misma licencia. **Estos perfiles no pueden venderse.**

---

*Hecho con paciencia, muchas impresiones fallidas, y mucho amor por la comunidad maker.*

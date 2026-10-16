# Axes Controller Board (ACB)

KiCad design files of the Axes Controller Board, the STM32H723-based motion
controller of the [DEDALUS](https://dedalus-5x.github.io/dedalus-docs/)
five-axis printer. The board drives three brushed DC servo axes through
external H-bridges and six stepper drivers in StepStick sockets, reads
differential incremental and SPI absolute encoders, and carries a
Raspberry Pi 5 as a daughterboard.

Firmware: <https://github.com/DEDALUS-5x/axes-controller-fw>

## Contents

| File | Description |
| --- | --- |
| `axes-controller-board.kicad_pro` | KiCad 8 project |
| `axes-controller-board.kicad_sch`, `mcu.kicad_sch`, `sensors.kicad_sch` | Schematics (root, MCU, sensors) |
| `axes-controller-board.kicad_pcb` | Four-layer layout, 203 x 108 mm, 1.6 mm |
| `libs/footprints.pretty/` | Footprints that can be redistributed with the design |
| `sym-lib-table` | Project symbol libraries (third-party, see below) |
| `outputs/ACB_schematic.pdf` | Schematic, PDF |
| `outputs/ACB_gerbers.zip` | Gerber and Excellon drill files (four layers) |
| `outputs/ACB_bom.csv` | BOM of the SMT-assembled parts, with LCSC codes |
| `outputs/ACB_positions.csv` | Placement of the SMT-assembled parts (top side, mm) |

The files in `outputs/` were exported from the KiCad project with `kicad-cli`
(KiCad 9). For JLCPCB assembly, the
[Fabrication Toolkit](https://github.com/bennymeg/Fabrication-Toolkit) plug-in
generates the BOM and placement files in the required format, using the LCSC
codes and rotation offsets stored in the schematic.

## Third-party library parts

The symbols, footprints and 3D models of the parts below are SamacSys ECAD
models or vendor models. Their terms allow using them in board designs
without restriction, but not redistributing them as library components, so
they are not included in this repository.

The schematics and the PCB embed a copy of every symbol and footprint they
use: the project opens and fabrication outputs can be generated without these
libraries. Download them only to edit the parts, free of charge, and place them
at the paths below, which match `sym-lib-table` and the footprint names in the
layout.

| Part | Use | Expected location | Source |
| --- | --- | --- | --- |
| STM32H723ZGT6 | IC5, microcontroller | `libs/STM32H7/STM32H723ZGT6/` | [Component Search Engine](https://componentsearchengine.com) (SamacSys) |
| TPS5450QDDARQ1 | IC1, 5 V buck converter | `libs/TPS5450/` | [Component Search Engine](https://componentsearchengine.com) (SamacSys) |
| ACS37800KMACTR-030B3-SPI | IC2-IC4, current monitors (optional) | `libs/ACS/` | [Component Search Engine](https://componentsearchengine.com) (SamacSys) |
| Phoenix Contact 1985205, 1985234, 1985250 | Terminal blocks | `libs/morsetto/`, `libs/footprints.pretty/`, `libs/3dmodels/` | [Component Search Engine](https://componentsearchengine.com) (SamacSys) |
| Samtec FTSH-107-01-L-DV-K-TR | J1, STDC14 debug connector | `libs/footprints.pretty/` | [Samtec](https://www.samtec.com) |
| Raspberry Pi 4 Model B | 3D model only | `libs/3dmodels/` | [Raspberry Pi Ltd](https://www.raspberrypi.com) |

## Revision 1.0.0 notes

On the prototype the following signals were re-routed with wires, because the
STEP lines of five driver sockets reach pins without timer outputs and the
Raspberry Pi chip select reaches a pin without the SPI3 hardware NSS function.
The firmware uses the re-routed pins.

| Signal | Destination | Routed to | Re-routed to |
| --- | --- | --- | --- |
| STEP Z1 | J9 pin 7 | PE0 | PB9 (TIM17 CH1) |
| STEP Z2 | J12 pin 7 | PE3 | PB5 (TIM3 CH2), J5 pin 7 |
| STEP A1 | J15 pin 7 | PD0 | PC12 (TIM15 CH1) |
| STEP A2 | J18 pin 7 | PD3 | PB6 (TIM16 CH1N), J5 pin 6 |
| STEP C | J21 pin 7 | PD11 | PC7 (TIM8 CH2), J4 pin 2 |
| Raspberry Pi CE0 | J27 pin 24 | PB1 | PA4 (SPI3 NSS) |

## License

Copyright 2026 Leonardo Pasquato.

This source describes Open Hardware and is licensed under the CERN-OHL-S v2.

You may redistribute and modify this source and make products using it under
the terms of the CERN-OHL-S v2 (<https://ohwr.org/cern_ohl_s_v2.txt>).

This source is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY,
INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A
PARTICULAR PURPOSE. Please see the CERN-OHL-S v2 for applicable conditions.

Source location: <https://github.com/DEDALUS-5x/axes-controller-board>

As per CERN-OHL-S v2 section 4, should You produce hardware based on this
source, You must where practicable maintain the Source Location visible on
the external case of the Gizmo or other products you make using this source.

Exceptions: `libs/footprints.pretty/MountingHole_3.2mm_M3_ISO14580.kicad_mod`
and `libs/footprints.pretty/raspberrypi.kicad_mod` are derived from the
[KiCad libraries](https://www.kicad.org/libraries/license/) and remain under
CC-BY-SA 4.0 with the KiCad library exception.

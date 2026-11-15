# UV curing box

A modular UV curing box for post-curing SLA-printed parts. It is built from
identical square LED boards, joined edge to edge by 3D-printed brackets.

It was designed for the open source nasopharyngeal swab
([uwo-fast/nasopharyngeal-swab](https://github.com/uwo-fast/nasopharyngeal-swab)),
whose resin swab heads it cures, and it was published with the swab in
Gallup et al. (2020), below. This repository holds its design files, split out of
the swab repository with their history.

## Contents

### `elec/`: the LED board

A KiCad 5 project (`UV_Curing_Box.pro`, `.sch`, `.kicad_pcb`, made with
pcbnew 5.1.5) and its fabrication output in `elec/Gerbers/`.

- A two-layer board, 80 x 80 mm.
- 12 LEDs in 5 mm through-hole footprints, as three strings of four from +12 V to
  ground, each string with one series resistor in a 1206 footprint.
- 4 two-pole terminal blocks (Phoenix MKDS 1.5), all in parallel across +12 V and
  ground, so power can come in on one and go on to the next board.
- 4 M2 mounting holes, 2.2 mm, each 5 mm in from a corner, where the brackets
  attach.

The schematic leaves the parts' values unset: the LEDs are `LED_g` and the
resistors `0`. The paper uses 395 nm UV LEDs and twelve boards, and gives the
whole curing system's cost as $45.42.

The symbols and footprints come from a library, `OCI_UPL`, that is not in this
repository. The board file carries its own footprints and `UV_Curing_Box-cache.lib`
carries the schematic symbols, so both open without it.

The Gerbers are dated 2019-08-26 and were exported by a KiCad 6.0 development build.

### `mech/`: the brackets and swab holder

`brackets.scad` is the OpenSCAD source for the brackets that join boards through
their mounting holes. Opened on its own it renders `e_2x2()`. Three of the STLs are
its modules, rendered at a finer resolution than the file's `$fn = 30`:

| STL | Module | What it joins | Size (mm) |
| --- | --- | --- | --- |
| `plate.stl` | `p_2x2()` | four boards meeting at their corners in one plane | 20.2 x 20.2 x 2 |
| `edge.stl` | `e_2x2()` | four boards along an edge of the box, two on each face | 20.2 x 13.7 x 13.7 |
| `corner.stl` | `corner()` | three boards at a corner of the box | 13.7 x 13.7 x 13.7 |

`legs.stl` (4 x 4.7 x 95 mm) and `top.stl` (74 x 50 x 2.5 mm) are the swab holder: four legs
and a top the swabs hang from. They have no source here.

## Build

The paper (sections 5 and 6.1) builds a 4 x 8 x 8 in box from 12 boards. Size a box
in whole boards, then count the parts from it.

What the paper's box takes:

| Part | Quantity |
| --- | --- |
| LED board (from `elec/Gerbers/`) | 12 |
| 5 mm through-hole UV LED, 395 nm | 12 per board |
| 2-pole screw terminal block | up to 4 per board |
| Edge bracket (`edge.stl`) | 12 |
| Corner bracket (`corner.stl`) | 8 |
| Plate bracket (`plate.stl`) | 2 |
| M2 x 10 mm screw | 48 |
| 12 V DC, 5 A power supply, and an IEC power cord | 1 |
| 18 AWG wire | 1 m |

The paper's bill of materials lists the screws as M3, which is an error: the board's
mounting holes are 2.2 mm (an M2 footprint) and the brackets' are 2.1 mm (`h_d` in
`brackets.scad`), sized for M2.

1. Lay the boards out unfolded, as the box opens flat, and decide which terminal
   blocks to fit so every board gets power at least once. Not every board needs all
   four.
2. Solder the LEDs on the side opposite the silkscreen, so they face into the box.
   LEDs are polarised. The paper runs them at full intensity: the resistor pads take
   a 0 ohm resistor, or a bridge cut from an LED lead.
3. Join the terminal blocks of boards that sit edge to edge in one plane with short
   lengths of clipped lead, clamped in both blocks. The blocks' polarity differs, and
   the board does not mark it: pin 1 is +12 V on J1 and J4 but ground on J2 and J3
   (`elec/UV_Curing_Box.net`). Check which pin is which before linking two blocks or
   wiring the supply.
4. Bolt the boards together with the brackets: plate where four boards meet in one
   plane, edge along an edge, corner at a corner. The paper threads the screws into
   the plastic from inside the box, with no nuts.
5. Wire the corner joints with insulated wire. Left long, these wires double as the
   lid's hinge.
6. Wire one terminal block to the supply's + and - terminals, and the IEC cord to its
   mains terminals. Check the cord's colour code before connecting it.
7. Before powering it, measure the box's input resistance. Around 5 ohm or less
   suggests a short.

## Use

Hang the parts to cure tip-down from the holder printed from `legs.stl` and `top.stl`,
set it in the box, close the lid and power the box. The paper cures swabs for 45 min.

## Origin and licence

The design is from:

> Gallup, N., Pringle, A. M., Oberloier, S., Tanikella, N. G., & Pearce, J. M.
> (2020). Parametric nasopharyngeal swab for sampling COVID-19 and other respiratory
> viruses: Open source design, SLA 3-D printing and UV curing system.
> *HardwareX*, 8, e00135. <https://doi.org/10.1016/j.ohx.2020.e00135>

Its files are also on the Open Science Framework, <https://osf.io/z5jgu/>.

The paper releases the design under the GNU General Public License v3.0 and the
CERN Open Hardware Licence v1.2. [`LICENSING.md`](LICENSING.md) says how they
apply here, and both texts are in [`LICENSES/`](LICENSES).

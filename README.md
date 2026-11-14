# UV curing box

A modular UV curing box for post-curing SLA-printed parts. It is built from
identical square LED boards, joined edge to edge by 3D-printed brackets.

It was designed for the open source nasopharyngeal swab
([uwo-fast/OpenNasopharyngealSwab](https://github.com/uwo-fast/OpenNasopharyngealSwab)),
whose resin swab heads it cures, and it was published with the swab in
Gallup et al. (2020), below. This repository holds its design files, split out of
the swab repository with their history.

## Contents

### `elec/`: the LED board

A KiCad 5 project (`UV_Curing_Box.pro`, `.sch`, `.kicad_pcb`, made with
pcbnew 5.1.5) and its fabrication output in `elec/Gerbers/`.

- A two-layer board, 80 × 80 mm.
- 12 LEDs in 5 mm through-hole footprints, as three strings of four from +12 V to
  ground, each string with one series resistor in a 1206 footprint.
- 4 two-pole terminal blocks (Phoenix MKDS 1.5), all in parallel across +12 V and
  ground, so power can come in on one and go on to the next board.
- 4 M2 mounting holes, 2.2 mm, each 5 mm in from a corner, where the brackets
  attach.

The schematic leaves the parts' values unset: the LEDs are `LED_g` and the
resistors `0`. Choose a UV LED, and the resistor for a string of four of them at
12 V, before ordering.
The symbols and footprints come from a library, `OCI_UPL`, that is not in this
repository. The board file carries its own footprints and `UV_Curing_Box-cache.lib`
carries the schematic symbols, so both open without it.

The Gerbers are dated 2019-08-26 and were exported by a KiCad 6.0 development build.

### `mech/`: the brackets and stand

`brackets.scad` is the OpenSCAD source for the brackets that join boards through
their mounting holes. Opened on its own it renders `e_2x2()`. Three of the STLs are
its modules, rendered at a finer resolution than the file's `$fn = 30`:

| STL | Module | What it joins | Size (mm) |
| --- | --- | --- | --- |
| `plate.stl` | `p_2x2()` | four boards meeting at their corners in one plane | 20.2 × 20.2 × 2 |
| `edge.stl` | `e_2x2()` | four boards along an edge of the box, two on each face | 20.2 × 13.7 × 13.7 |
| `corner.stl` | `corner()` | three boards at a corner of the box | 13.7 × 13.7 × 13.7 |

`legs.stl` (4 × 4.7 × 95 mm) and `top.stl` (74 × 50 × 2.5 mm) have no source here.

## Origin and licence

The design is from:

> Gallup, N., Pringle, A. M., Oberloier, S., Tanikella, N. G., & Pearce, J. M.
> (2020). Parametric nasopharyngeal swab for sampling COVID-19 and other respiratory
> viruses: Open source design, SLA 3-D printing and UV curing system.
> *HardwareX*, 8, e00135. <https://doi.org/10.1016/j.ohx.2020.e00135>

Its files are also on the Open Science Framework, <https://osf.io/z5jgu/>.

This repository is under the GNU General Public License v3.0, the licence these
files were under in the swab repository. See [`LICENSE`](LICENSE).

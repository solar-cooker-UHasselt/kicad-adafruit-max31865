# kicad-adafruit-max31865

[![ERC/DRC](https://img.shields.io/github/actions/workflow/status/solar-cooker-UHasselt/kicad-adafruit-max31865/kibot.yml?branch=main&event=push&label=ERC%2FDRC)](https://github.com/solar-cooker-UHasselt/kicad-adafruit-max31865/actions/workflows/kibot.yml)
[![KiCad 10](https://img.shields.io/badge/KiCad-10-314CB0)](https://www.kicad.org/download/)

A breakout board for the MAX31865, an amplifier and converter for PT100 platinum RTD
temperature probes. It has a 430 Ω reference resistor, solder jumpers for 2, 3 or 4 wire
probes, a screw terminal for the probe, and a 3.3 V regulator and level shifting so it
works with 3.3 V and 5 V boards. It is a KiCad 10 port of
[Adafruit's MAX31865 breakout](https://github.com/adafruit/Adafruit-MAX31865-PCB),
originally drawn in Eagle.

In the [solar cooker](https://github.com/solar-cooker-UHasselt) testing station, the
firmware ([arduino-code](https://github.com/solar-cooker-UHasselt/arduino-code)) reads
the temperature of three 4-wire PT100 probes over SPI, one board per probe.

[![3D render of the board, top side](https://solar-cooker-uhasselt.github.io/kicad-adafruit-max31865/MAX31865-render-top.png)](https://solar-cooker-uhasselt.github.io/kicad-adafruit-max31865/)

- [Board page](https://solar-cooker-uhasselt.github.io/kicad-adafruit-max31865/): 3D
  renders, drawings, schematic and board PDF, interactive BOM and the BOM CSV for
  Eurocircuits, rebuilt by CI on every push to `main`
- [PCB source](https://github.com/adafruit/Adafruit-MAX31865-PCB): Adafruit's Eagle
  files
- [Adafruit guide](https://learn.adafruit.com/adafruit-max31865-rtd-pt100-amplifier):
  pinout, wiring, the jumpers for 2, 3 or 4 wires and Arduino examples

## Installation

### Requirements

- [KiCad 10](https://www.kicad.org/download/), to open and edit the board
- [git](https://git-scm.com), to clone the repo
- Optional, for the checks: [just](https://just.systems)

### Clone

```bash
git clone https://github.com/solar-cooker-UHasselt/kicad-adafruit-max31865.git
```

Own symbols, footprints and 3D models are in `external/`, in the repo itself.

## Usage

Open `MAX31865.kicad_pro` in KiCad 10. The schematic and board open from there.

Before committing, run ERC and DRC, reports in `tmp/`:

```bash
just check
```

`just --list` shows the other recipes: PDFs, and the CI workflow run locally.

## Contributing

Branches, pull requests, approval and commits work as in every repo of the org, see
[CONTRIBUTING.md](https://github.com/solar-cooker-UHasselt/.github/blob/main/.github/CONTRIBUTING.md).
Specific to this repo:

- CI runs ERC and DRC on every pull request, through the shared workflows in
  [kicad-ci](https://github.com/solar-cooker-UHasselt/kicad-ci). A push to `main` that
  changes more than Markdown also rebuilds the board page.
- Types, scopes and the ERC/DRC check for commits are in
  [.agents/commit_conventions.md](.agents/commit_conventions.md).
- Changes to the board setup, CI or docs are made and tested in
  [kicad-adafruit-ds3231](https://github.com/solar-cooker-UHasselt/kicad-adafruit-ds3231)
  first, then copied here.

## Authors and acknowledgment

The board is Adafruit's design, by Limor Fried/Ladyada for Adafruit Industries
([PCB source](https://github.com/adafruit/Adafruit-MAX31865-PCB)). The KiCad 10 port is
by Lowie Van Vyve for the UHasselt solar cooker project.

Adafruit invests time and resources providing this open source design. Please support
Adafruit and open-source hardware by purchasing products from
[Adafruit](https://www.adafruit.com).

<details>
<summary>Adafruit's original README, included as its license asks</summary>

> **Adafruit MAX31865 PCB**
>
> [Click here to purchase one from the Adafruit shop](http://www.adafruit.com/products/3328)
>
> PCB files for the Adafruit MAX31865 RTD Sensor Breakout. Format is EagleCAD schematic
> and board layout
>
> - https://www.adafruit.com/product/3328
>
> **Description**
>
> For precision temperature sensing, nothing beats a Platinum RTD. Resistance
> temperature detectors (RTDs) are temperature sensors that contain a resistor that
> changes resistance value as its temperature changes, basically a kind of thermistor.
> In this sensor, the resistor is actually a small strip of Platinum with a resistance
> of 100 ohms at 0°C, thus the name PT100. Compared to most NTC/PTC thermistors, the PT
> type of RTD is much most stable and precise (but also more expensive) PT100's have
> been used for many years to measure temperature in laboratory and industrial
> processes, and have developed a reputation for accuracy (better than thermocouples),
> repeatability, and stability.
>
> However, to get that precision and accuracy out of your PT100 RTD you must use an
> amplifier that is designed to read the low resistance. Better yet, have an amplifier
> that can automatically adjust and compensate for the resistance of the connecting
> wires. If you're looking for a great RTD sensor, today is your lucky day because we
> have a lovely Adafruit RTD Sensor Amplifier with the MAX31865 breakout for use with
> any 2, 3 or 4 wire PT100 RTD!
>
> **License**
>
> Adafruit invests time and resources providing this open source design, please support
> Adafruit and open-source hardware by purchasing products from
> [Adafruit](https://www.adafruit.com)!
>
> Designed by Limor Fried/Ladyada for Adafruit Industries.
>
> Creative Commons Attribution/Share-Alike, all text above must be included in any
> redistribution. See license.txt for additional details.

</details>

## License

[CC BY-SA 4.0](LICENSE). Adafruit's original design is licensed CC BY-SA 3.0, which
allows adaptations like this port under a later version with the same terms.

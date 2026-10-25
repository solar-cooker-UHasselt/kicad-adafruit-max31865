# AGENTS.md

Guidance for AI coding agents working in this repository. `CLAUDE.md` is a symlink to
this file.

Make sure to also read [`README.md`](README.md) for the human-facing docs.

## Project overview

KiCad 10 port of the Adafruit MAX31865 breakout, an amplifier and converter for PT100
platinum RTD temperature probes, part of the UHasselt solar cooker testing station.
There is no code: the repo is a KiCad project, checked by KiBot in CI through the shared
workflows in [kicad-ci](https://github.com/solar-cooker-UHasselt/kicad-ci).

Changes to the board setup, CI or docs are made and tested in the reference repo,
[kicad-adafruit-ds3231](https://github.com/solar-cooker-UHasselt/kicad-adafruit-ds3231),
first, then copied here.

Not here yet, unlike ds3231: the `kicad-common` submodule (own parts are still in
`external/`), the Eurocircuits design rules (`.kicad_dru`) and Renovate.

## Repository map

```
MAX31865.kicad_pro        # project: board setup, pinned libraries, revision
MAX31865.kicad_sch        # schematic
MAX31865.kicad_pcb        # board layout
drawing_sheet.kicad_wks   # title block and logos, this repo's own copy
sym-lib-table             # symbol library: external_library from external/
fp-lib-table              # footprint library: external_footprints from external/
external/                 # own symbols, footprints and 3D models
justfile                  # ERC, DRC, PDFs, local CI
.github/workflows/        # thin wrappers calling kicad-ci at @v1
.github/CODEOWNERS        # who approves pull requests
.agents/                  # commit and documentation conventions
```

## Where things live

| To change…                                   | Edit                                                 |
| -------------------------------------------- | ---------------------------------------------------- |
| The circuit                                  | `MAX31865.kicad_sch` in KiCad, then F8 to the board  |
| Placement, routing, zones                    | `MAX31865.kicad_pcb` in KiCad                        |
| An own symbol, footprint or 3D model         | `external/`, in KiCad's symbol and footprint editors |
| ERC/DRC in CI, the board page, KiBot outputs | the `kicad-ci` repo, then move its `v1` tag          |
| Which kicad-ci workflows run, and when       | `.github/workflows/`                                 |

The maintainer edits the schematic and board in KiCad. Agents read the files, run the
checks, and change text files (`sym-lib-table`, `.kicad_pro` fields, workflows, docs).

## Setup

Nothing to set up after the clone: the own parts are in `external/`. Needs KiCad 10
(`kicad-cli`) and just. `just ci` also needs act and Docker.

## Checks

```bash
just check   # ERC and DRC with schematic parity, reports in tmp/
just ci      # the KiBot workflow locally with act, for workflow changes
```

Done when: ERC 0 errors, DRC 0 errors, 0 parity issues. Compare the counts with the
previous commit. `just ci` caches kicad-ci's `v1`: after the tag moves, clear it with
`rm -rf ~/.cache/act/solar-cooker-UHasselt-kicad-ci*`.

Before finishing, re-read the diff and check it follows existing patterns, is minimal
(nothing unrelated touched), and passes `just check`. (Our convention.)

## Tools

- **KiBot, through kicad-ci.** No `config.kibot.yml` here: kicad-ci's default config
  applies. The KiBot image is pinned in kicad-ci, not here.
- **kicad-happy.** Use its `kicad` skill for a design review of the schematic and
  board. House rules it does not know are in this file and `.agents/`.
- **Lint, through kicad-ci.** `.github/workflows/lint.yml` runs actionlint, with
  ShellCheck on the `run:` scripts, when a file in `.github/workflows/` changes.

## Security

- No secrets: CI uses only the `GITHUB_TOKEN`, with `pages: write` and `id-token: write`
  for the board page.
- Never commit `*.kicad_prl`, lock files (`~*`), `tmp/` or generated outputs.
  `.gitignore` covers them.

## Commit & PR guidelines

**Never commit. Propose the message only**, and the maintainer reviews and commits
manually. Do not run `git commit` / `git push` / `git add` unless explicitly told to.
(Our convention.)

Use the `propose-commit` skill: Conventional Commits, one concern per commit, the
message handed over in `tmp/`. This repo's types, scopes and check:
[.agents/commit_conventions.md](.agents/commit_conventions.md). **Never use em dashes or
semicolons** in commit messages or any written output. (Our convention.)

Pull requests follow the org's
[CONTRIBUTING.md](https://github.com/solar-cooker-UHasselt/.github/blob/main/.github/CONTRIBUTING.md),
enforced by the org's
[rulesets](https://github.com/solar-cooker-UHasselt/.github/tree/main/rulesets).

## Documentation

Use the `check-docs` skill when writing or reshaping a README, a guide in `docs/` or an
agent file: one Diátaxis type per file, the README shape, naming. This repo's doc map,
exceptions and link check:
[.agents/documentation_conventions.md](.agents/documentation_conventions.md).
(Our convention.)

## Deployment

- The board page (renders, drawings, schematic and board PDF, interactive BOM, BOM
  CSV) is published to GitHub Pages by the `pages` job in `.github/workflows/kibot.yml`,
  from `main` only: every push that changes more than Markdown, and manual runs. Pull
  requests never publish.
  <https://solar-cooker-uhasselt.github.io/kicad-adafruit-max31865/>
- A pushed tag like `v1.2` makes a draft release of that order, by
  `.github/workflows/release.yml`. The tag must match the board revision. Only the
  maintainer tags and publishes. How it works: kicad-ci's README, "Release an order".

## References

- agents.md spec: <https://agents.md/>
- KiCad file formats: <https://dev-docs.kicad.org/en/file-formats/>
- Adafruit's original design (Eagle): <https://github.com/adafruit/Adafruit-MAX31865-PCB>

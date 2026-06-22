# Repository Guidelines

## Project Structure & Module Organization
This repository contains a ZMK user configuration and custom board definition for the `eyelash_sofle` keyboard.

- `boards/arm/eyelash_sofle/`: board DTS, defconfig, layout, Kconfig, and board metadata.
- `config/`: active ZMK configuration files, including `eyelash_sofle.keymap`, `.conf`, `.json`, and `west.yml`.
- `mise.toml`: local tool and task definitions for drawing keymaps and native firmware builds.
- `keymap-drawer/`: generated keymap YAML/SVG outputs used in the README.
- `.github/workflows/`: GitHub Actions for firmware builds and keymap drawing.
- `build.yaml`: build matrix used by the ZMK user-config workflow.
- `.build/`: ignored local west workspace and firmware build output.

## Build, Test, and Development Commands
Prefer the repo-local `mise` tasks for repeatable local work. Firmware builds also run in GitHub Actions on push or `workflow_dispatch`.

- `git status`: check pending local changes before editing keymap or board files.
- `mise install`: install declared local tools from `mise.toml`.
- `mise run draw`: regenerate `keymap-drawer/eyelash_sofle.yaml` and `.svg`.
- `mise run build-left` / `mise run build-right`: build the standard firmware targets.
- `mise run build-studio-left`: build the left half with ZMK Studio enabled.
- `mise run build-all`: build every target listed in `build.yaml`.

For CI, `.github/workflows/build.yml` calls ZMK's `build-user-config.yml`. `.github/workflows/draw.yml` regenerates keymap drawings when `config/**` or drawer configuration changes.

## Coding Style & Naming Conventions
Keep ZMK and Zephyr naming consistent with existing files: lowercase board names such as `eyelash_sofle_left`, DTS files ending in `.dts`/`.dtsi`, and Kconfig files named `Kconfig.board` or `Kconfig.defconfig`. Use two-space indentation in YAML. Preserve the existing keymap formatting and align related bindings for readability.

## Testing Guidelines
There is no standalone unit test suite. Validate firmware changes by building the affected halves and checking GitHub Actions results. For keymap edits, run `mise run draw` and confirm the regenerated `keymap-drawer` files are intentional.

## Commit & Pull Request Guidelines
Recent history uses short, direct messages such as `Updated eyelash_sofle.keymap`; automated draw commits are prefixed with `[Draw]`. Use imperative, scoped messages, for example `Update eyelash_sofle keymap` or `Adjust right half power config`.

Pull requests should describe the hardware or keymap behavior changed, mention which halves were built, and include screenshots or regenerated SVGs when visual keymap output changes. Link related issues when available.

## Agent-Specific Instructions
Avoid rewriting generated files unless the source keymap or drawer config changed. Treat board definitions, modifier layers, and power settings carefully, because small changes can affect flashing, pairing, battery behavior, or OS-specific shortcuts.

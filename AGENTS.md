# Repository Guidelines

## Project Structure & Module Organization
This repository contains a ZMK user configuration and custom board definition for the `eyelash_sofle` keyboard.

- `boards/arm/eyelash_sofle/`: board DTS, defconfig, layout, Kconfig, and board metadata.
- `config/`: active ZMK configuration files, including `eyelash_sofle.keymap`, `.conf`, `.json`, and `west.yml`.
- `keymap-drawer/`: generated keymap YAML/SVG outputs used in the README.
- `.github/workflows/`: GitHub Actions for firmware builds and keymap drawing.
- `build.yaml`: build matrix used by the ZMK user-config workflow.
- `sofle-3D-MODEL.zip`: hardware asset; do not modify unless intentionally updating the model package.

## Build, Test, and Development Commands
Firmware builds are normally run by GitHub Actions on push or manually with `workflow_dispatch`.

- `git status`: check pending local changes before editing keymap or board files.
- `west update`: fetch ZMK and module dependencies when building locally from a Zephyr workspace.
- `west build -p -b eyelash_sofle_left -- -DSHIELD=nice_view`: example local left-side firmware build.
- `west build -p -b eyelash_sofle_right -- -DSHIELD=nice_view`: example local right-side firmware build.

For CI, `.github/workflows/build.yml` calls ZMK's `build-user-config.yml`. `.github/workflows/draw.yml` regenerates keymap drawings when `config/**` or drawer configuration changes.

## Coding Style & Naming Conventions
Keep ZMK and Zephyr naming consistent with existing files: lowercase board names such as `eyelash_sofle_left`, DTS files ending in `.dts`/`.dtsi`, and Kconfig files named `Kconfig.board` or `Kconfig.defconfig`. Use two-space indentation in YAML. Preserve the existing keymap formatting and align related bindings for readability.

## Testing Guidelines
There is no standalone unit test suite. Validate changes by building both keyboard halves and checking GitHub Actions results. For keymap edits, confirm `keymap-drawer/eyelash_sofle.svg` renders correctly after the draw workflow or local keymap-drawer run.

## Commit & Pull Request Guidelines
Recent history uses short, direct messages such as `Updated eyelash_sofle.keymap`; automated draw commits are prefixed with `[Draw]`. Use imperative, scoped messages, for example `Update eyelash_sofle keymap` or `Adjust right half power config`.

Pull requests should describe the hardware or keymap behavior changed, mention which halves were built, and include screenshots or regenerated SVGs when visual keymap output changes. Link related issues when available.

## Agent-Specific Instructions
Avoid rewriting generated files unless the source change requires it. Treat board definitions and power settings carefully, because small DTS or Kconfig changes can affect flashing, pairing, or battery behavior.

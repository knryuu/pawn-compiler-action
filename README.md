# Pawn Compiler Action

Compiles Pawn scripts using the open.mp compiler in GitHub Actions workflows.

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

## Features

- 🚀 Compiles `.pwn` scripts to `.amx` bytecode
- ⚙️ Supports both prebuilt binaries and source compilation
- 📦 Outputs compiled file path for artifact upload
- 🔧 Configurable compiler flags and include paths
- 💻 Cross-platform support (Linux runner)

## Usage

### Basic Compilation
```yaml
steps:
- uses: actions/checkout@v4

- name: Compile Pawn script
  id: compiler
  uses: knryuu/pawn-compiler-action@v1
  with:
    input: gamemodes/main.pwn
    includes: pawno/include

- name: Upload compiled artifact
  uses: actions/upload-artifact@v4
  with:
    name: compiled-script
    path: ${{ steps.compiler.outputs.compiled_file }}
```

### Full Configuration
```yaml
- name: Compile with custom settings
  id: pawn-compile
  uses: knryuu/pawn-compiler-action@v1
  with:
    input: filterscripts/anticheat.pwn
    output: filterscripts/anticheat.amx
    flags: '-\\;+ -\\(+ -d3'
    includes: 'pawno/include my-includes'
    source_url: 'https://github.com/pawn-lang/compiler'
```

### Install Compiler Only
```yaml
- name: Install compiler
  id: pawncc
  uses: knryuu/pawn-compiler-action@v1

- name: Run custom commands
  run: |
    ${{ steps.pawncc.outputs.pawncc }} --version
    ${{ steps.pawncc.outputs.pawncc }} myscript.pwn -iincludes
```

## Input Parameters

| Parameter       | Required | Default          | Description |
|-----------------|----------|------------------|-------------|
| `input`         | No       | `"gamemodes/main.pwn"`             | Path to .pwn source file |
| `output`        | No       | `"-Dinput_path"`             | Custom output path for compiled file |
| `flags`         | No       | `"-\\;+ -\\(+ -d3"`  | Compiler flags (space separated [use secape string for `-;+` and `-(+`]) |
| `includes`  | No       | `"-ipawno/include"` | Include paths (space separated) |
| `source_url`    | No       | `""`             | Source repository URL for custom builds (defaults to download prebuilt open.mp compiler latest release) |

## Output Values

| Output          | Description |
|-----------------|-------------|
| `pawncc`        | Path to installed compiler binary |
| `compiled_file` | Path to compiled .amx file (empty if not compiled) |

## Error Handling

The action will fail with descriptive errors for:
- Missing compiler binary
- Compilation failures
- Missing input files
- Invalid paths

## License
This project is licensed under the GNU Affero General Public License v3.0. See [LICENSE](LICENSE) for full text.

## Support
For issues and feature requests, please [open an issue](https://github.com/knryuu/pawn-compiler-action/issues).

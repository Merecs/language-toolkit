# Merecs Language Toolkit for LabVIEW

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
![LabVIEW 2019+](https://img.shields.io/badge/LabVIEW-2019%2B-yellow.svg)
![Platform: Windows](https://img.shields.io/badge/Platform-Windows-lightgrey.svg)

Translate the complete user interface of a LabVIEW application at run time,
including Unicode languages such as Russian or Chinese.

The toolkit has two parts:

- **Language Editor**: a development tool that works like a dictionary. You pick the
  VIs to translate, and the editor lists every control and indicator on their front
  panels. You enter the translations for each language, and the editor stores them in
  a JSON configuration file.
- **Language Selection** (run-time library): a set of VIs you call from your application.
  At startup it loads the configuration file and applies the translations to all
  registered front panels. It can also switch languages while the application runs.

## Features

- ANSI (western) and Unicode languages in the same application
- Startup language and fallback (default) language
- Translates front panels of VIs, including cloned/reentrant VIs registered at run time
- Translates string constants on the block diagram (`Translate text.vi`)
- Recurses into clusters, tab controls and arrays
- Translation status for each control in the editor (not added / untranslated / partially translated / translated)
- Import and export as CSV for external translators

### Supported elements

| LabVIEW element | Translatable parts |
| --- | --- |
| String control | Caption, string value, tip strip |
| Boolean control | Caption, Boolean text, tip strip |
| Ring, Combo Box | Caption, item strings, tip strip |
| Array (1D, 2D) | Caption, string elements, tip strip |
| Listbox, Multicolumn Listbox | Caption, item names, headers, tip strip |
| Table | Caption, items, headers, tip strip |
| Tab control | Caption, page names, tip strip |
| Graphs and charts | Caption, X/Y scale names, plot names (ASCII only), cursors, tip strip |
| String constant | Value |
| All other elements | Caption, tip strip |

## Requirements

- LabVIEW 2019 or later (the source is saved in LabVIEW 2019), Windows
- Unicode support enabled in LabVIEW: add `UseUnicode=TRUE` to `LabVIEW.ini`, and to the
  `.ini` file of every executable you build
- These VI packages, installed with [VIPM](https://www.vipm.io/):
  - [JKI State Machine](https://www.vipm.io/package/jki_lib_state_machine/)
  - [JSONtext](https://www.vipm.io/package/jdp_science_jsontext/) (JDP Science)
  - [NI Unicode Tools](https://www.vipm.io/package/ni_lib_unicode/) (`ni_lib_unicode`)

## Installation

### From a release (recommended)

1. Download the latest `.vip` file from the [Releases](../../releases) page.
2. Double-click it to install it with VIPM. VIPM also installs the required dependencies.
3. Start the Language Editor from the LabVIEW menu
   **Tools » Merecs Engineering » Merecs Language Toolkit » Language Editor...**.
4. Place the Language Selection VIs from the **Functions** palette
   **Addons » Merecs Language Toolkit**.

### From source

1. Clone this repository.
2. Open [`Merecs Language Toolkit.vipc`](Merecs%20Language%20Toolkit.vipc) with VIPM and click **Apply**.
   This installs all dependencies in the tested versions.
3. Open `src/Merecs Language Toolkit.lvproj` in LabVIEW 2019 or later.
4. Run `src/Language Editor/Language Editor.vi` directly, and use the VIs from
   `src/Language Selection`. The **Tools** menu entry and the **Addons** palette are only
   created when you install the package.

## Language Editor or Language Selection?

| | Language Editor (`Language Editor.lvlib`) | Language Selection (`Language Selection.lvlib`) |
| --- | --- | --- |
| Purpose | Create and edit translations | Apply translations in your application |
| Used by | You, the developer, during development | Your application at run time |
| Part of your built application | No | Yes |
| Output / input | Writes the translation file (JSON) | Reads the translation file (JSON) |

You call only the VIs of **Language Selection** from your code. The **Language Editor** is a
tool you run separately; your application does not need it.

## Quick start

1. Start the Language Editor (see [Installation](#installation)). Set **Application Base Folder
   Path** to the base folder of your project, and **Language Config Path** to the translation
   file. Then add the languages you need on the **Languages** page.
2. On **VIs to Translate**, add the VIs of your application and enter the translations.
3. Click **Save** to write the translations to the JSON file.
4. In your application, call `Init.vi` from the Language Selection library at startup:
   - **Language Translation file**: path to the JSON file from step 3
   - **Top Level VI (Caller)**: reference to your main VI. Only VIs in its hierarchy are translated;
     for VIs loaded dynamically by path, place a static VI reference in the main VI.
   - **Clone VIs** (optional): references to dynamically launched clones that are already running

   `Init.vi` loads the file and registers the front panels to translate.
5. Call `Translate front panels.vi` once after `Init.vi` to apply the startup language,
   and again whenever the user selects another language.
   Use `Translate text.vi` for strings you build on the block diagram.
   For clones launched after `Init.vi`, call `Add CTRLs By VI Reference.vi`.

`Init.vi` does not look for the file itself; your application decides where it is stored.
When you build an executable, include the translation file in the build and pass its path,
for example relative to the application directory.

See `src/Examples` for a working example: `Example_Simple.vi` and its translation file
`Simple_Translations.json`.

The [user manual](docs/manual.md) describes the Language Editor, the translation format for
each element type, and all public VIs in detail.

## Repository layout

| Path | Content |
| --- | --- |
| `src/Language Selection` | Run-time library you call from your application |
| `src/Language Editor` | Development tool for creating translations |
| `src/Examples` | Example VIs and the translation file they use |
| `packages/error-generation` | Error Generation library, used by the Language Selection library to create errors |
| `docs/` | [User manual](docs/manual.md) and maintainer documentation, such as the [release process](docs/RELEASING.md) |

## Contributing

Bug reports, feature requests and pull requests are welcome. Read
[CONTRIBUTING.md](CONTRIBUTING.md) before you start. This project follows the
[Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).

To report a security issue, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.

## License

Copyright 2020-2026 Merecs Engineering GmbH

Licensed under the [Apache License, Version 2.0](LICENSE). Third-party components and their
licenses are listed in [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

LabVIEW is a trademark of National Instruments Corp. (NI). This project is not affiliated with
or endorsed by NI.

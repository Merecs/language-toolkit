# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
project uses [Semantic Versioning](https://semver.org/). VIPM package versions add a fourth
number for the build (`major.minor.patch.build`).

## [Unreleased]

## [1.0.1] - 2026-10-30

### Added

- Unicode language support for export and import.

### Fixed

- Removed extra quotation marks from Ring elements during import.
- Preserved untranslated controls in the JSON when changing the language.

## [1.0.0] - 2026-09-28

First public release under the Apache License 2.0.

### Added

- Language Editor for creating and editing translations, with CSV import and export.
- Language Selection library for applying translations at run time, including Unicode
  languages and language switching while the application runs.
- Example `Example_Simple.vi` with its translation file.
- User manual (`docs/manual.md`).

[Unreleased]: https://github.com/Merecs/language-toolkit/compare/v1.0.1...HEAD
[1.0.1]: https://github.com/Merecs/language-toolkit/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/Merecs/language-toolkit/releases/tag/v1.0.0

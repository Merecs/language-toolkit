# Releasing

This page is for maintainers. It describes how to build the VI package and publish a release.

## Package identity

| Setting | Value |
| --- | --- |
| Build spec file | `Merecs Language Toolkit.vipb` in the repository root |
| Package name | `merecs_engineering_lib_merecs_language_toolkit` |
| Display name | Merecs Language Toolkit |
| Vendor | Merecs Engineering GmbH |
| License | Apache-2.0 (license file: `LICENSE`) |
| LabVIEW version | 2019, 32-bit and 64-bit |
| Source folder | Repository root (`.`) |
| Output folder | `builds/` (ignored by git) |
| Resulting file | `builds/merecs_engineering_lib_merecs_language_toolkit-<version>.vip` |
| Dependency file | `Merecs Language Toolkit.vipc` in the repository root (for developers) |

Keep the package name the same for every release. VIPM uses it to recognize upgrades, and a
different name creates a separate package.

### Install locations

| Content | Destination |
| --- | --- |
| `src/Language Selection`, `src/Language Editor` | `<vi.lib>\Merecs\Language Toolkit\` |
| `packages/error-generation/src` | `<vi.lib>\Merecs\Language Toolkit\support\` |
| `src/Examples` | `<examples>\Merecs Engineering\Merecs Language Toolkit\Examples\` |
| `docs/manual.md`, `docs/images/` | `<vi.lib>\Merecs\Language Toolkit\docs\` |

Exclude these from the package: `.github/`, `docs/RELEASING.md`, `builds/`, all `.lvproj`
files, and `packages/installed.json`.

In the package's *Help* or *Documentation* link, point to the manual on GitHub
(`https://github.com/Merecs/language-toolkit/blob/main/docs/manual.md`). There the images
are shown correctly.

### Dependencies to add in the build spec

| Package | Minimum version |
| --- | --- |
| `jki_lib_state_machine` | 2018.0.7.45 |
| `jdp_science_jsontext` | 1.9.1.125 |
| `ni_lib_unicode` | 2.0.1.6 |

The minimum versions are those used during development and listed in
`Merecs Language Toolkit.vipc`. Keep both in sync. Lower them only after testing.

`Merecs Language Toolkit.vipc` also lists `jdp_science_lib_common_utilities` (installed together with
JSONtext) and `jki_rsc_toolkits_palette` (palette for the JKI State Machine). They do not
need to be added to the build spec separately.

### Metadata

- **Summary**: Translate the user interface of LabVIEW applications at run time, including Unicode languages.
- **Description**: Use the first paragraphs of the README.
- **Home page / Repository URL**: the GitHub repository URL
- **Release notes**: copy the section for this version from `CHANGELOG.md`
- **Icon**: a package icon that shows the toolkit (not the default VIPM icon)

## Versioning

The project uses Semantic Versioning (`major.minor.patch`). VIPM adds a build number as the
fourth part. Use the same version in:

- the VIPM build spec
- `NI.Lib.Version` of `Language Selection.lvlib` and `Language Editor.lvlib`
- the git tag (`v1.2.3`)
- the heading in `CHANGELOG.md`

## Release checklist

1. Move the **Unreleased** entries in `CHANGELOG.md` to a new version section.
2. Update the library versions and the build spec version.
3. Mass compile `src/` and `packages/error-generation` in LabVIEW 2019 and fix broken VIs.
4. Build the package with VIPM.
5. Install the package on a clean LabVIEW 2019 installation (32-bit and 64-bit), and run
   the examples from the Example Finder.
6. Commit, tag (`git tag v1.2.3`) and push the tag.
7. Create a GitHub release for the tag. Attach the `.vip` file and paste the changelog section.

## Publishing on vipm.io (later)

vipm.io accepts packages that are free of charge, include the source code, and use an
OSI-approved license. Apache-2.0 meets all three. Publishing requires an approved
[VIPM Community Publisher](https://www.vipm.io/publish/) account.

Before applying:

1. Publish at least one stable release on GitHub and collect feedback.
2. Make sure all library names are unique (see below).
3. Apply at <https://www.vipm.io/publish/>. After approval, upload each new `.vip` under
   *My Published Packages*.

### Library names must be unique

LabVIEW identifies libraries by their qualified name. Two libraries with the same name cannot
be in memory at the same time. `Error Generation.lvlib` in `packages/error-generation` has a
generic name that other packages may also use. Rename it with a `Merecs` prefix, or replace it,
before the first public release, because renaming it later breaks code that depends on it.

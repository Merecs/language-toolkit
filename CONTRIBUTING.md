# Contributing to the Merecs Language Toolkit

Thank you for helping. This page explains how to report problems and submit changes.

## Reporting bugs and requesting features

Open an [issue](../../issues/new/choose) and pick the matching template. For bugs, include:

- LabVIEW version and bitness (for example LabVIEW 2021 SP1, 64-bit)
- Toolkit version (VIPM package version or commit hash)
- The control type and language involved (ANSI or Unicode)
- Steps to reproduce it, ideally with a small example VI

## Development setup

1. Install LabVIEW 2019 or later. The source is saved in **LabVIEW 2019**. Do not save VIs
   in a newer version, because that stops users on older versions from using the code.
2. Open `Merecs Language Toolkit.vipc` in the repository root with VIPM and click **Apply** to
   install the dependencies. If you add or update a dependency, update this file too
   (VIPM: File > Save As VI Package Configuration).
3. Add `UseUnicode=TRUE` to your `LabVIEW.ini`.
4. Open `src/Merecs Language Toolkit.lvproj`.

## Submitting changes

1. Fork the repository and create a branch from `main`, for example `fix/tab-control-unicode`.
2. Keep each pull request focused on one change. VIs are binary files that git cannot
   merge, so small pull requests are much easier to review.
3. Before you commit:
   - Turn on **Separate compiled code from source file** for new VIs and controls
     (VI Properties > General). This keeps diffs small.
   - Do not commit VIs that only changed because LabVIEW recompiled them. Revert those.
   - Fill in the VI description (VI Properties > Documentation) for public VIs.
   - Run the examples in `src/Examples` to check that nothing broke.
4. In the pull request, describe what changed and why. For UI changes, add a screenshot of
   the front panel or block diagram, because reviewers cannot diff VIs on GitHub.
5. Add an entry under **Unreleased** in [CHANGELOG.md](CHANGELOG.md).

To compare VIs, use LabVIEW's **Compare VIs** tool (Tools > Compare > Compare VIs), or
configure LVCompare as your git diff tool.

## Coding conventions

- Put public VIs in the library root folder and helper VIs in `private/`, and set their
  access scope to *private* in the `.lvlib`.
- Use type definitions (`.ctl`) for clusters and enums that cross VI boundaries.
- Use the icon style of the library you are working in.
- Name VIs in English with words separated by spaces, for example `Translate text.vi`.

## License of contributions

By submitting a pull request you agree that your contribution is licensed under the
[Apache License 2.0](LICENSE), as described in section 5 of the license. Only submit code
that you wrote yourself or that is available under a compatible license, and say so in the
pull request if it includes third-party code.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).

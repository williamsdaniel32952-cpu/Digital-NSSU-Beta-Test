# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-30

### Added
- Initial public release of the NSSU Input Portal.
- PWA manifest, service worker, and 192/512 icons for installable / offline use.
- README, CHANGELOG, LICENSE, and `.gitignore`.

### Changed
- **Suggestion / guide text now clears the moment a step or rework box is focused.** Previously the blue placeholder text only changed color to black on focus and remained in the box, so typed characters were appended to the suggestion. Now clicking into any of the Summary, Step 1–4, Rework 1–4, Key Points, or combined-text-field boxes immediately empties the box if it was holding suggestion or manual-guide text. Boxes that already contain user-entered content are left alone.

### Hidden
- **Import from Work Instruction** menu button is hidden from the dropdown via inline `display:none`. The importer code, file input, and event wiring remain in place so the feature can be brought back by removing one attribute when a functional model is ready.

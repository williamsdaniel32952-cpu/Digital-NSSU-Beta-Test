# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2026-05-02

### Changed
- **Library parser no longer injects standard-fallback prose into the loaded library object** when `STANDARD_FALLBACK_ENABLED` is false. Previously, every library import added a hardcoded `standardFallback.step1Template` and `step2` to the in-memory override regardless of the flag — unreachable at runtime, but visually present in the data model. The injection is now gated on the same flag.
- **Existing localStorage library overrides are sanitized on load.** When the app starts, if `STANDARD_FALLBACK_ENABLED` is false, any `standardFallback` field and any populated `baseBehavior` table on the cached library override are cleared and the cleaned object is written back to storage. One-and-done per device.
- Service worker `CACHE_VERSION` bumped to `nssu-v1.1.1`.

## [1.1.0] - 2026-05-02

### Changed
- **Standard fallback rework language is now disabled.** A new `STANDARD_FALLBACK_ENABLED` feature flag (set to `false`) gates the `buildStandardFallbackRule` path inside `buildReworkRule`. When the user selects a defect-and-tool combination that does not exist in the approved library, the four rework boxes now show the blue manual-guide steps for the user to write themselves, instead of generating constructed fallback prose. The fallback function, the `builtInBaseBehavior` table, and the `libraryOverride.standardFallback` / `libraryOverride.baseBehavior` slots are left in place — flip the flag to `true` to re-enable the path with no other changes required.
- Service worker `CACHE_VERSION` bumped to `nssu-v1.1.0` so installed clients pick up the new `index.html` on next visit.

## [1.0.0] - 2026-04-30

### Added
- Initial public release of the NSSU Input Portal.
- PWA manifest, service worker, and 192/512 icons for installable / offline use.
- README, CHANGELOG, LICENSE, and `.gitignore`.

### Changed
- **Suggestion / guide text now clears the moment a step or rework box is focused.** Previously the blue placeholder text only changed color to black on focus and remained in the box, so typed characters were appended to the suggestion. Now clicking into any of the Summary, Step 1–4, Rework 1–4, Key Points, or combined-text-field boxes immediately empties the box if it was holding suggestion or manual-guide text. Boxes that already contain user-entered content are left alone.

### Hidden
- **Import from Work Instruction** menu button is hidden from the dropdown via inline `display:none`. The importer code, file input, and event wiring remain in place so the feature can be brought back by removing one attribute when a functional model is ready.

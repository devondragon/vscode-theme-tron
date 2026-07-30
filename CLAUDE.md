# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A declarative VS Code color theme extension. There is **no source code, no build step, and no runtime** — the entire extension is `package.json` plus two JSON theme files. Editing a theme means editing hex values in `themes/*.json`.

## Commands

```bash
npm run package    # vsce package -> tron-legacy-theme-<version>.vsix (gitignored)
npm run publish    # vsce publish  -> VS Code Marketplace (publisher: DigitalSanctuary)
```

`vsce` is not a devDependency; it must be installed globally (`npm i -g @vscode/vsce`). There are no dependencies and no `node_modules`.

`npm test` is a placeholder that exits 1 — there is no test suite. Verify changes visually instead: `npm run package`, then `code --install-extension tron-legacy-theme-<version>.vsix`, reload, and pick the theme via `Preferences: Color Theme`.

## Structure

- `package.json` — extension manifest. `contributes.themes[]` binds each marketplace-visible `label` to a file `path`. The `uiTheme` there (`vs-dark` / `vs`) must match the `type` field (`dark` / `light`) inside the corresponding theme JSON.
- `themes/tron-legacy-color-theme.json` — dark variant (`"type": "dark"`)
- `themes/tron-legacy-light-color-theme.json` — light variant (`"type": "light"`)
- `images/` — `icon.png` plus the two README screenshots referenced by `package.json.screenshots`

## Theme file conventions

Both theme files are deliberately **structurally parallel**: the same 12 `colors` keys and the same 9 `tokenColors` entries, in the same order, with the same `name` and `scope` arrays. Only the hex values differ between light and dark. When adding a color key or a token rule, add it to **both** files in the same position, or the variants drift.

One known asymmetry: the dark theme's "Variables" rule includes the scope `support.type.property-name.json`, the light theme's does not.

The `colors` map covers only 7 UI areas (editor, activityBar, sideBarTitle, titleBar, statusBar, terminal). Everything else is inherited from VS Code's built-in `vs-dark`/`vs` base theme — that minimalism is intentional, so adding a key means opting that element out of the base theme permanently.

Neither theme defines `semanticTokenColors` or sets `semanticHighlighting`, so semantic highlighting falls back to TextMate `tokenColors`.

## Releasing

1. Bump `version` in `package.json`.
2. Add a `## [x.y.z] - YYYY-MM-DD` entry to `CHANGELOG.md`.
3. `npm run package` and verify the `.vsix`, then `npm run publish`.

Note: `package.json` contains a stray `__metadata` block (size, `installedTimestamp`, `source: "vsix"`) left over from a local VSIX install. It is not part of the extension manifest spec and can be removed.

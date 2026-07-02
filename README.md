# renovate-config
Shared Renovate configuration presets for bright-room repositories, referenced via `extends`.

## Usage

Extend the base preset plus one preset per language used in the repository:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>bright-room/renovate-config",
    "github>bright-room/renovate-config:java"
  ]
}
```

`github>bright-room/renovate-config` resolves to `default.json` (the base preset), and
`github>bright-room/renovate-config:<name>` resolves to `<name>.json`.

Multi-language repositories (e.g. Tauri apps, monorepos) extend every applicable language preset.

## Presets

| Preset | File | Contents |
|---|---|---|
| base | `default.json` | Schedule (Saturday before 9am JST), labels, no PR limits, `separateMinorPatch`, mise enabled, 7-day minimum release age, automerge (major: 14 days, manual merge), pinned GitHub Action digests |
| `java` | `java.json` | Groups Spring Boot updates |
| `kotlin` | `kotlin.json` | Groups Kotlin monorepo and Spring Boot updates |
| `go` | `go.json` | `gomodTidy`, groups Go toolchain and `golang.org/x` updates |
| `rust` | `rust.json` | Groups non-major cargo updates and Tauri updates, lock file maintenance |
| `typescript` | `typescript.json` | Pins npm dependency versions, `pnpmDedupe`, groups TypeScript/@types, React, Testing Library, Vite/Vitest, Electron |
| `terraform` | `terraform.json` | Lock file maintenance, tracks `terraform_version` in GitHub workflow files |

Framework-specific rules (Spring Boot, React, Electron, Tauri) live inside the relevant language
preset: grouping rules only take effect when the matching packages are present, so repositories
that don't use those frameworks are unaffected.

### Examples

| Repository type | `extends` |
|---|---|
| Spring Boot (Java) | `[base, java]` |
| Spring Boot (Kotlin) | `[base, kotlin]` |
| Go | `[base, go]` |
| Rust CLI | `[base, rust]` |
| Tauri app | `[base, rust, typescript]` |
| Electron app | `[base, typescript]` |
| Terraform | `[base, terraform]` |

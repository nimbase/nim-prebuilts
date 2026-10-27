# nim-prebuilts

Community Nim toolchain prebuilts for Linux, macOS and Windows on x64 and arm64, tracking nim-lang/nim releases.

## Assets

Each `vX.Y.Z` release follows `nim-lang.org` naming for unix, plus `windows_` prefix on Windows for consistency (upstream ships `nim-X.Y.Z_x64.zip` with no OS part):

| Runner | Asset |
| --- | --- |
| `ubuntu-24.04` | `nim-X.Y.Z-linux_x64.tar.xz` |
| `ubuntu-24.04-arm` | `nim-X.Y.Z-linux_arm64.tar.xz` |
| `macos-15-intel` | `nim-X.Y.Z-macosx_x64.tar.xz` |
| `macos-14` | `nim-X.Y.Z-macosx_arm64.tar.xz` |
| `windows-latest` | `nim-X.Y.Z-windows_x64.zip` |
| `windows-11-arm` | `nim-X.Y.Z-windows_arm64.zip` |

Unix archives contain a top-level `nim-X.Y.Z/` directory (same as upstream), Windows zips contain `nim-X.Y.Z/` with `bin/nim.exe`. `sha256sums.txt` is attached to every release.

## How it works

`nim-lang/nim` does not notify this repo on new tags, so `.github/workflows/build.yml` polls:

- `schedule: cron 0 6 * * *` resolves `stable` from `nim-lang.org/channels/stable` (fallback: latest `v*` tag via `git ls-remote`) and skips if `vX.Y.Z` already exists here.
- `workflow_dispatch` with optional `nim-version` (exact, e.g. `2.2.12`, or `stable`) builds one version on demand.

Each matrix job clones `nim-lang/Nim@vX.Y.Z`, runs `build_all.sh` (unix) or `build_all.bat` (windows), packages with the names above, and uploads an artifact. The `release` job merges artifacts and runs `gh release create vX.Y.Z`.

## Manual build

Actions tab -> build -> Run workflow -> `nim-version: 2.2.12`.

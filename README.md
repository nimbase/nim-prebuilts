# nim-prebuilts

Mirror of [nim-lang.org/releases.json](https://nim-lang.org/releases.json) for
reliable CI downloads, consumed by
[nimbase/setup-nim-action](https://github.com/nimbase/setup-nim-action).

A single evergreen release [`latest`](https://github.com/nimbase/nim-prebuilts/releases/tag/latest)
holds one asset, `releases.json` — a byte-copy of upstream. It is updated in
place (never a new tag per Nim version).

Stable URL:

```text
https://github.com/nimbase/nim-prebuilts/releases/download/latest/releases.json
```

Consumers must always use the `github_url` entries (hosted on
`github.com/nim-lang/nightlies`), not `nimlang_url`, because GitHub is more
reliable than `nim-lang.org/download`.

## Coverage

Upstream builds (see
[nightlies.yml](https://github.com/nim-lang/nightlies/blob/master/.github/workflows/nightlies.yml))
cover `linux_x64`, `linux_arm64`, `linux_armv7l`, `linux_x32`,
`macosx_x64`, `macosx_arm64`, `windows_x64`, `windows_x32`, plus `source_tar`.
There is no `windows_arm64` build upstream. Some versions may miss a key while
builds roll out (e.g. `2.4.0` shipped without `linux_arm64` initially); treat a
missing key as "fall back".

## How it works

`.github/workflows/mirror.yml` polls weekly:

- `schedule: cron 0 6 * * 0` fetches upstream, validates JSON, compares
  sha256 with the current `latest` asset, and re-uploads with `--clobber` only
  on change (release notes record date + upstream hash).
- `workflow_dispatch` with `force: true` re-uploads even if unchanged.

## Manual mirror

Actions tab -> mirror -> Run workflow -> `force: true`.

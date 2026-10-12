# ci-assets

Versioned third-party files that DramaClaw CI needs and can no longer download
from their original source. Each file is published as a release asset; this
repository holds its checksum and license notice.

Nothing here is built or modified by DramaClaw. Every file is the unmodified
upstream binary, and CI verifies its SHA-256 before using it.

## Contents

| Asset | Version | Platform | Release tag | Notices |
|---|---|---|---|---|
| Ladybug JSON extension | 0.16.0 | linux_amd64 | `ladybug-json-0.16.0` | [`ladybug-json/0.16.0`](ladybug-json/0.16.0) |

### Ladybug JSON extension 0.16.0

- File: `libjson-linux_amd64.lbug_extension`, 846,920 bytes
- SHA-256: `0767a3326207849596669810166cf10f843b6985ebbfcd917a05b8e8359ee028`
- Origin: downloaded on 2026-08-21 from
  `https://extension.ladybugdb.com/v0.16.0/linux_amd64/json/libjson.lbug_extension`
- License: MIT. It statically links yyjson, also MIT. Both notices are in
  [`ladybug-json/0.16.0/LICENSE.txt`](ladybug-json/0.16.0/LICENSE.txt).

Why it is here: DramaClaw pins `ladybug==0.16.0`. Ladybug loads its JSON
extension from `~/.lbdb/extension/<version>/<platform>/json/` and downloads it
on first use when it is missing. The upstream extension server stopped hosting
v0.16.0, so that download now returns 404 and tests that open a real Ladybug
database fail. CI places this file in the cache directory before running tests.

## Using an asset

```sh
curl -fsSL -o libjson.lbug_extension \
  https://github.com/dramaclaw/ci-assets/releases/download/ladybug-json-0.16.0/libjson-linux_amd64.lbug_extension
echo "0767a3326207849596669810166cf10f843b6985ebbfcd917a05b8e8359ee028  libjson.lbug_extension" | sha256sum -c -
```

Pin the SHA-256 in the consuming workflow. Do not trust the download alone.

## Adding an asset

1. Add `<name>/<version>/SHA256SUMS` and `<name>/<version>/LICENSE.txt` with the
   upstream notices, and a row in the table above with the origin URL and date.
2. Publish a release tagged `<name>-<version>` and attach the unmodified file.
   Attach `SHA256SUMS` and `LICENSE.txt` to the release as well, so the notice
   travels with the binary.
3. Never replace an asset in place. A changed file gets a new tag.

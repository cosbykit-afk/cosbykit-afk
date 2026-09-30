# cosbykit-afk

**Lampy** — a self-hosted, all-in-one forum stack in a single container:
PostgreSQL 16, TimescaleDB, pgvector, Apache, Ollama (local LLM),
Apache James (mail), code-server, and a Flask forum app.

## Repositories

| Repo | What it is |
|---|---|
| [lampy-single](https://github.com/cosbykit-afk/lampy-single) | The stack itself: Dockerfile, build scripts, forum app |
| [lampy-installer](https://github.com/cosbykit-afk/lampy-installer) | Windows installer (WSL2-based, downloads the system image) |
| [lampy-deps](https://github.com/cosbykit-afk/lampy-deps) | Build dependencies: Python wheelhouse + Ollama mirror |

## Assembling the pieces

Large files are split into parts on GitHub Releases (2 GB per-file
limit). Download **all** parts for the archive you need, then
concatenate them **in order** before extracting.

### Build dependencies ([lampy-deps](https://github.com/cosbykit-afk/lampy-deps), release `build-deps-v1`)

**Python wheelhouse** (3.3 GB, 3 parts):

```bash
cat wheelhouse.tar.part00 wheelhouse.tar.part01 wheelhouse.tar.part02 > wheelhouse.tar
tar -xf wheelhouse.tar        # produces wheelhouse/
mv wheelhouse wheelhouse      # place beside the Dockerfile
```

**Ollama distribution mirror** (5 GB, 4 parts — `/usr/bin/ollama` and
`/usr/lib/ollama` from the pinned `ollama/ollama:latest` image):

```bash
cat ollama-donor.tar.part00 ollama-donor.tar.part01 \
    ollama-donor.tar.part02 ollama-donor.tar.part03 > ollama-donor.tar
tar -xf ollama-donor.tar      # produces ollama-donor/
```

### System image ([lampy-installer](https://github.com/cosbykit-afk/lampy-installer), release `v1.0.0`)

**Lampy system image** (11 GB, 7 parts). The Windows installer
(`install.ps1`) downloads and verifies these automatically against
`lampy-public.tar.sha256`. To assemble manually:

```powershell
# Download all 7 parts + the .sha256 file from the release, then:
Get-Content lampy-public.tar.part-* -Raw -AsByteStream |
    Set-Content lampy-public.tar -AsByteStream
# Verify:
Get-FileHash lampy-public.tar.part-* -Algorithm SHA256
# (compare against lampy-public.tar.sha256)
```

## License notes

- [Terms of Use](https://www.facebook.com/permalink.php?story_fbid=pfbid0EYdC7t3nnkshMjssisYwyix5p8JLyD3Ns7EvEkRP4ugNtB8z8dKcrBwdJKSqLKaPl&id=61594635330402)

- This account's original code is public domain ([The Unlicense](https://unlicense.org)).
- The stack bundles third-party components under their own licenses;
  see `COMPONENT_LICENSES.md` in lampy-single.
- **TimescaleDB**: the Timescale License permits free self-hosted use
  at any scale, but you may not offer TimescaleDB itself as a hosted
  database service. See `FEES.md` in lampy-single for the commercial
  breakdown (spoiler: license fees are $0 across the stack).

## System architecture

Text-based SAD documentation (Mermaid) for the whole ecosystem, kept current
as features are added:

- [System-wide context diagram and database documentation](docs/system.md) —
  all 12 repositories as subsystems, external entities, data flows between
  them, the data-store inventory, and cross-system entity relationships.
- Per-repo docs, each with a context diagram, a level-1 data flow diagram,
  and an entity–relationship diagram:
  - [Bible](https://github.com/cosbykit-afk/Bible/blob/main/docs/architecture.md)
  - [R-Theory](https://github.com/cosbykit-afk/R-Theory/blob/main/docs/architecture.md)
  - [bible-project](https://github.com/cosbykit-afk/bible-project/blob/main/docs/architecture.md)
  - [euclid-proofs](https://github.com/cosbykit-afk/euclid-proofs/blob/main/docs/architecture.md)
  - [genealogy](https://github.com/cosbykit-afk/genealogy/blob/main/docs/architecture.md)
  - [gwen-training](https://github.com/cosbykit-afk/gwen-training/blob/main/docs/architecture.md)
  - [lampy-admin](https://github.com/cosbykit-afk/lampy-admin/blob/main/docs/architecture.md)
  - [lampy-deps](https://github.com/cosbykit-afk/lampy-deps/blob/main/docs/architecture.md)
  - [lampy-installer](https://github.com/cosbykit-afk/lampy-installer/blob/main/docs/architecture.md)
  - [lampy-single](https://github.com/cosbykit-afk/lampy-single/blob/main/docs/architecture.md)
  - [lampy-temp-backup](https://github.com/cosbykit-afk/lampy-temp-backup/blob/main/docs/architecture.md)
  - [r-theory-rewrite](https://github.com/cosbykit-afk/r-theory-rewrite/blob/main/docs/architecture.md)

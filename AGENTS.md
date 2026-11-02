# LGRoot

LGRoot contains SlopBro, a Python launcher and webOS payloads for rooting LG TVs
through the jsserver vulnerability. See `README.md` for usage and `COPYING` for
the AGPL-3.0-or-later license.

## Stack and layout

- `slopbro.py`: Python 3 launcher, HTTP server, SSAP/WebSocket client, and asset
  loading. Keep the launcher standard-library-only.
- `wwwroot/`: HTML, JavaScript, and shell payloads. Its `package.json` is a webOS
  service manifest; this is not an npm project.
- `tools/package_single_file.py`: embeds payloads into a standalone launcher;
  requires Python 3.12 or newer.
- `tests/test_slopbro.py`: standard-library unittest regression suite.
- `Makefile`: packages the standalone launcher into `dist/`.

## Build, test, and run

Run from the repository root with Python 3. On Windows, `python` or `py -3` can
replace `python3`; use Python 3.12 or newer for packaging.

```sh
python3 slopbro.py --help
python3 -m unittest discover -s tests -v
python3 tools/package_single_file.py --out dist/slopbro_packed.py
```

- When changing required payload files, keep the asset lists in `slopbro.py`,
  `tools/package_single_file.py`, and `Makefile` aligned.
- Preserve both directory-based and embedded asset loading. After launcher or
  payload changes, check the packaged launcher as well as the source version.
- Local tests exercise CLI and handler behavior without a TV. Report hardware
  verification separately; passing local tests does not establish TV compatibility.
- Pairing credentials (`*.key`) and generated packages (`dist/`) stay ignored.
- No formatter, linter, type checker, or CI workflow is configured in this repo.

## Session continuity

Memhub at `.memhub/project.sqlite` is the source of truth for project memory.
At session start, read `.memhub/rendered/PROJECT.md` once if present, unless the
session-start hook already supplied its frame. If it is missing, use
`memhub status` and `memhub recall`.

Use `memhub recall` for decisions, facts, tasks, and architecture details before
reading the full ledger. Use `memhub locate` to find code by intent.
`memhub arch show` prints the full architecture narrative.

Retrieval uses hybrid search with reranking. Drive sync is disabled; keep memory
local unless the user explicitly changes that choice. `.memhub/` and rendered
memory files stay ignored by Git.

Route session updates through `/wrap-up` and regenerate views with `memhub render`;
edit memory through memhub rather than editing generated Markdown.

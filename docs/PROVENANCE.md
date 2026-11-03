# Provenance and integrity

This document records where this repository comes from and how to verify a clone.
Checked on 2026-11-03 via the GitHub API.

## Fork chain

- `brewedcode/ARTEX` is a GitHub fork (`fork: true`) whose **parent is `BaaRai/ARTEX`**
  (`source`/`parent` in the API both point to `BaaRai/ARTEX`).
- `BaaRai/ARTEX` is **not** itself a registered fork (`fork: false`, no parent). Its README
  describes it as an English translation of `Autumn-27/ARTEX` (the Chinese original).
- The upstream repository `Autumn-27/ARTEX` **no longer exists** (HTTP 404). The `Autumn-27`
  account still exists (ScopeSentry, Norma, katana, ...), but ARTEX is not among its repos.
  Consequence: the link to the original cannot be verified through GitHub fork metadata
  anymore; it rests on the README claim plus archived copies (e.g. web.archive.org
  snapshots of `github.com/Autumn-27/ARTEX`, and the Go module proxy cache of
  `github.com/!autumn-27/artex`, if still present).

## License

AGPL-3.0 (`LICENSE`).

## History rewrite (2026-10-23)

The git history was rewritten on 2026-10-23 to remove a personal email address from
commit metadata. All commit hashes from before that date are invalid, and clones made
before it cannot fast-forward (`update.sh` detects this and refuses to rebuild stale
code). To verify that a clone contains the rewritten history:

```bash
git log --reverse --format='%ad' -1   # root commit date must be on/after 2026-10-23
```

A CI job (`history-hygiene` in `.github/workflows/ci.yml`) now rejects new commits whose
author/committer email is a personal address, so the problem cannot recur.

## Dependencies

- `go.mod` requires `github.com/Autumn-27/norma v0.4.4` **directly from upstream**; there
  is no `replace` directive anymore. The upstream tag `v0.4.4` of `Autumn-27/Norma` points
  to the same git object (`7714f3d6901aa5428e24ee4cde7e1e72a6c4b7bd`) as the previously used
  `BaaRai/Norma v0.4.4`, so the code is identical — only the source is now the upstream repo.

## Container images

- The ARTEX image is **built from this checkout only** (`pull_policy: build` in
  `docker-compose.yml` / `docker-compose.bench.yml`). Never pull a prebuilt `artex` image:
  the old default `autumn27/artex:latest` pointed at a Docker Hub namespace whose GitHub
  repository is gone, so whoever controls that name could publish a malicious tag.
- Only `postgres:16-alpine` (official upstream image) is pulled.

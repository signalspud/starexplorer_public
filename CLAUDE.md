# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

StarExplorer Public is a **public issue tracker** for the private `signalspud/starexplorer` repository. It contains no application source code — only a GitHub Actions workflow that mirrors issues and comments from this public repo to the private one.

## Repository Structure

- `.github/workflows/sync_to_private.yml` — The entire project logic: a single GitHub Actions workflow (235 lines of shell scripting)
- `.github/empty.md` — Placeholder file

There is no build system, test framework, or linting. All logic lives in the workflow YAML.

## Workflow Architecture

The workflow (`sync_to_private.yml`) triggers on `issues` and `issue_comment` events and runs a single `sync` job with these sequential steps:

1. **Extract private issue number** — Parses the public issue body for `<!-- mirrored-private: N -->` HTML comments to find the linked private issue
2. **Create private issue** — If no mirror link exists, creates a new issue in `signalspud/starexplorer` via the GitHub API
3. **Update private issue** — If a mirror link exists, patches the private issue with the latest title/body
4. **Sync labels** — Replaces private issue labels with the public issue's current labels
5. **Sync state** — Mirrors open/closed state to the private issue
6. **Mirror comments** — Copies new comments to the private issue with SHA256 hash-based deduplication and loop protection

## Key Patterns

- **Retry helper**: Every API call uses a `retry()` shell function (3 attempts, 2s delay). This function is redefined in each step.
- **HTML comment metadata**: Issue bodies contain `<!-- mirrored-private: N -->` (on public) and `<!-- mirrored-public: N -->` (on private) to link mirrored pairs.
- **Comment dedup**: Each mirrored comment includes a `<!-- mirror-hash: SHA256 -->` marker. Before posting, existing comments are checked for hash matches.
- **Loop protection**: Comments prefixed with `**Private comment by @...` are skipped to prevent infinite mirroring loops.
- **Authentication**: All private repo API calls use the `PRIVATE_STARS_TOKEN` secret.
- **JSON construction**: Uses `jq -n` to safely build JSON payloads (avoids injection via issue titles/bodies).

## Working on the Workflow

When editing `sync_to_private.yml`:
- The workflow uses `set -euo pipefail` in every step — any unhandled failure will abort
- GitHub Actions expressions (`${{ }}`) are used inline in shell scripts; be careful with quoting to avoid injection
- Test changes by creating/editing issues on the public repo and checking the Actions tab and the private repo
- The `extract` step output (`steps.extract.outputs.private`) gates whether subsequent steps run as creates or updates

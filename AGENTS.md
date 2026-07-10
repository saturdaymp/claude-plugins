# AGENTS.md

Non-obvious constraints for working in this repo. For what the repo is and how releases work, see [README.md](README.md).

- `scripts/generate-changelog.sh` and `plugins/smp-github/skills/smp-create-gh-changelog-script/generate-changelog.sh` are intentionally identical: the skill template is the source of truth, and the `scripts/` copy is that skill's output applied to this repo. Apply any fix to both files.
- When adding or changing a skill, update all three doc levels: the skill's `SKILL.md`, the plugin's `README.md`, and the skill links in the root `README.md`.
- Plugin manifests (`plugin.json`) must NOT declare a `version` — it silently pins both release channels to the same version (see "Versioning and Releases" in the README).
- `main` has a branch ruleset requiring changes via pull request; pushes to `main` will be rejected. Releases are cut only by running the Release workflow.
- No automated tests. Verify skill changes by running the skill end to end (a real PR review comment URL, or a repo with GitHub releases).
- Skills use phase-based workflows with explicit **STOP** points awaiting user confirmation. Never remove these gates — they are intentional for user safety.

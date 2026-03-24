# SaturdayMP Claude Skills

[![Sponsor](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=fe8e86)](https://github.com/sponsors/saturdaymp)

Custom skills for the Claude Code agent that Saturday Morning Productions finds useful.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated

## Installation

### From GitHub

Add this repo as a plugin marketplace, then install:

```
/plugin marketplace add saturdaymp/claude-skills
/plugin install claude-skills@saturdaymp/claude-skills
```

### From a local clone

```bash
git clone https://github.com/saturdaymp/claude-skills.git
```

Then in Claude Code:

```
/plugin marketplace add /path/to/claude-skills
/plugin install claude-skills@claude-skills
```

## Skills

### smp-fix-pr-review

Fixes, addresses, or responds to a GitHub PR review comment. Given a review comment URL, it will:

1. Fetch the comment and its thread
2. Evaluate whether the feedback is valid
3. Propose and implement a fix (with your approval at each step)
4. Commit the change
5. Reply to the comment and resolve the conversation

**Usage:**

```
/smp-fix-pr-review https://github.com/owner/repo/pull/123#discussion_r1234567
```

You can also describe what you want naturally — e.g., "fix this PR review comment" and paste the URL.

### smp-generate-changelog

Creates a standalone bash script (`scripts/generate-changelog.sh`) that generates a `CHANGELOG.md` from GitHub releases. The script can be run independently without Claude.

**Usage:**

```
/smp-generate-changelog
```

Once the script is created, you can run it directly:

```bash
./scripts/generate-changelog.sh                    # Writes to CHANGELOG.md in repo root
./scripts/generate-changelog.sh docs/CHANGES.md    # Custom output path
```

**What the script does:**

1. Verifies that `gh` is installed and authenticated
2. Detects the GitHub repository from the current git working directory
3. Fetches all published (non-draft) releases, sorted by date (newest first)
4. Generates a formatted Markdown changelog with version headings, dates, and release notes
5. Includes link references to each release on GitHub

Pre-releases are included and marked with a `(Pre-release)` label in the heading.

**Script requirements:**

- Must be run from inside a git repository that has a GitHub remote
- [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated
- `jq` (bundled with `gh`)

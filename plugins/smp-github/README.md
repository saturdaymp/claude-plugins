# smp-github

A Claude Code plugin with skills for common GitHub workflows.

## Installation

Add the marketplace, then install the plugin (see the [repo README](../../README.md#installation) for details and release channels):

```
/plugin marketplace add saturdaymp/claude-plugins
/plugin install smp-github@saturdaymp-claude-plugins
```

## Skills

### smp-apply-github-pr-feedback

Fixes, addresses, or responds to a GitHub PR review comment. Given a review comment URL, it will:

1. Fetch the comment and its thread
2. Evaluate whether the feedback is valid
3. Propose and implement a fix (with your approval at each step)
4. Commit the change
5. Reply to the comment and resolve the conversation

**Usage:**

```
/smp-apply-github-pr-feedback https://github.com/owner/repo/pull/123#discussion_r1234567
```

You can also describe what you want naturally — e.g., "fix this PR review comment" and paste the URL.

**Requirements:** [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated.

### smp-create-gh-changelog-script

Creates a standalone bash script, `scripts/generate-changelog.sh`, that generates a `CHANGELOG.md` from your repository's GitHub releases. Because the script is plain bash, changelog generation keeps working without Claude — run it by hand or from CI. The skill will:

1. Check prerequisites (git repository, `gh` CLI)
2. Write the script to `scripts/generate-changelog.sh` in your repository
3. Optionally run it to generate the initial `CHANGELOG.md`
4. Optionally add usage documentation to your README
5. Optionally commit the new files (with your approval)

**Usage:**

```
/smp-create-gh-changelog-script                  # Changelog written to CHANGELOG.md
/smp-create-gh-changelog-script docs/CHANGES.md  # Custom changelog path
```

Natural language also works — e.g., "generate a changelog from my GitHub releases".

**Requirements:** [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated, and a repository with at least one GitHub release.

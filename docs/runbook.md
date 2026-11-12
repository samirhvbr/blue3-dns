# Runbook — Blue3 DNS

> **Status:** `ACTIVE` · Documentation maintenance only. No DNS deployment is defined.

## 1. Requirements

A Markdown viewer is sufficient to read the guides. Git is needed to contribute.
Bash and GitHub CLI are used by the inherited release tooling; Python 3 can be
used for local metadata checks. There are no application dependencies to install.

## 2. Checkout

```bash
git clone https://github.com/samirhvbr/blue3-dns.git
cd blue3-dns
git config core.hooksPath tools/git-hooks
```

For an existing clone, inspect `git status --short` before `git pull --ff-only`.
Preserve work in progress and resolve divergence deliberately.

## 3. Documentation checks

Read changed pages as a user would, follow every changed relative link, and
check Mermaid labels and tables in a Markdown preview. Confirm that external
product claims link to their official source and carry a verification date.

```bash
git diff --check
python3 -m json.tool .claude/settings.json >/dev/null
bash -n tools/git-hooks/commit-msg tools/git-hooks/pre-push tools/release.sh
diff <(tail -n +2 CLAUDE.md) <(tail -n +2 AGENTS.md)
```

Check that local links resolve, the release workflow parses as YAML, version.md
matches the latest changelog heading, and the staged files contain no identifying
profiles or query data. There is no application suite, device test, or network
benchmark in this repository.

## 4. Publication

Commit with the current changelog heading as its subject and push to master.
The Release workflow publishes the version tag and GitHub Release on a hosted
runner. Inspect its result and confirm that the remote version, tag commit, and
Latest release agree.

```bash
gh run list --repo samirhvbr/blue3-dns --workflow release.yml --limit 3
gh release view --repo samirhvbr/blue3-dns
```

If the workflow cannot run, diagnose it first. A manual
`./tools/release.sh --current --repo samirhvbr/blue3-dns` is a recovery path only
after confirming no workflow publisher is running. This publishes documentation;
it does not install DNS settings or operate a resolver.

## 5. The git hooks

Once per clone — yours, and every collaborator's:

```bash
git config core.hooksPath tools/git-hooks
```

Check the hook directly, without creating commits. Run from the repository
root, outside a merge, with the hook's escape variable unset. `version.md` must
already be in the index; stage it during initial bootstrap. The accepted subject
uses that indexed version, including when a different version is unstaged.

```bash
(
  set -euo pipefail
  hook_version="$(git show :version.md | grep -oE '[0-9]+\.[0-9]+\.[0-9]+' | sed -n '1p')"
  test -n "$hook_version"
  hook_other_version=0.0.0
  if [ "$hook_version" = "$hook_other_version" ]; then hook_other_version=0.0.1; fi
  hook_message="$(mktemp)"
  trap 'rm -f "$hook_message"' EXIT

  printf '%s\n' 'feat: a hook validation subject' > "$hook_message"
  if bash tools/git-hooks/commit-msg "$hook_message"; then
    echo 'ERROR: the hook accepted a Conventional Commits prefix' >&2
    exit 1
  fi
  printf '%s - a hook validation subject\n' "$hook_other_version" > "$hook_message"
  if bash tools/git-hooks/commit-msg "$hook_message"; then
    echo 'ERROR: the hook accepted a version absent from the index' >&2
    exit 1
  fi
  printf '%s - a hook validation subject\n' "$hook_version" > "$hook_message"
  bash tools/git-hooks/commit-msg "$hook_message"
  echo 'commit-msg: expected rejection and acceptance checks passed; no commit created'
)
```

The hooks use `BLUE3_DNS_NO_HOOK` as their documented emergency escape variable.
Rules: [versioning.md](versioning.md).

## 6. Pre-flight before making a repository public

This repository is public from its initial publication. Review every delivery:

- [ ] `git log -p | grep -iE 'password|secret|token|api[_-]?key'` — scan the
      **history**, not just the working tree. A secret removed from HEAD is
      still in every clone.
- [ ] Any secret ever committed has been **rotated**, not merely deleted.
- [ ] No internal hostname, private IP range or infrastructure path that should
      not be public.
- [ ] `LICENSE` is present and `NOTICE` agrees with it about the license and the
      copyright holder.
- [ ] `SECURITY.md` names a reporting address that is actually monitored.
- [ ] Every prescriptive document declares its status; nothing reads as current
      while describing something abandoned.
- [ ] `.continue/` tracks pending decisions and execution, not completed work.
      Reviewable design/pilot proposals may live in `docs/` when marked
      `PROPOSED` and linked from the queue, as recorded in
      [ADR-004](decisions.md#adr-004--focus-the-proposal-on-blue3-subscribers).
- [ ] `README.md` is in English and describes what the project **is**, not what
      it was going to be.
- [ ] No placeholder left over from the skeleton:
      `grep -rn '<[A-Z_]\+>' . --exclude-dir=.git`

## 7. Verifying the repository still conforms

```bash
# the twins are identical below the H1
diff <(tail -n +2 CLAUDE.md) <(tail -n +2 AGENTS.md)

# version.md is a bare X.Y.Z
grep -qE '^[0-9]+\.[0-9]+\.[0-9]+$' version.md && echo ok

# settings.json parses
python3 -m json.tool .claude/settings.json > /dev/null && echo ok

# every version in history has a tag and a Release (prints what is missing)
./tools/release.sh --backfill --dry-run
```

If the `diff` on the twins reports anything other than the mirroring comment,
an edit was applied to one file and not the other.

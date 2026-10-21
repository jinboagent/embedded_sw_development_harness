---
name: prepublish-audit
description: Run the pre-publish privacy/secrets audit over a repository branch before it goes public - token-format greps, credential-assignment greps, personal-info patterns (names, emails, home paths, LAN IPs, MACs), and git-history checks. Use whenever a repo or branch is about to be pushed somewhere public or shared outside the team.
---

# Pre-publish Audit

A clean repo private can be a leaking repo public. Run every check below
against the exact ref that will be pushed; each command must return
NOTHING (or only documentation placeholders).

## When to use

- Before `git push` of any public-targeted repo/branch.
- Before releasing an archive/zip of a repo to outsiders.
- Periodically on an already-public repo after merges.

## 1. Credential token formats (cloud/API/VCS)

```
git grep -I -n -E "LTAI[0-9A-Za-z]+|AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9]{20,}|github_pat_|xox[bp]-|AIza[0-9A-Za-z_-]{20,}|sk-[A-Za-z0-9]{16,}|BEGIN [A-Z ]*PRIVATE KEY" <REF>
```

## 2. Key/secret/password assignments

```
git grep -I -n -iE "(api[_-]?key|secret|token|password|passwd|credential)[\"' ]*[:=][\"' ]*[A-Za-z0-9+/]{8,}" <REF>
```
Filter out placeholders (`example`, `YOUR_`, `<...>`, `${`, `xxxx`)
manually — inspect every survivor.

## 3. Personal information

```
git grep -I -n -iE "<username>|<real-name>|C:[/\\]Users|/Users/|/home/" <REF>
git grep -I -n -E "[A-Za-z0-9._%+-]+@[A-Za-z0-9-]+\.(com|cn|net|org|edu)" <REF>
git grep -I -n -E "\b(192\.168\.|10\.[0-9]+\.|172\.(1[6-9]|2[0-9]|3[01])\.)[0-9]+\.[0-9]+\b" <REF>
git grep -I -n -E "\b([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}\b" <REF>   # MACs: only obvious placeholders may match
```

## 4. History checks (all refs — the branch is only half the story)

```
git log --all --oneline -- <secrets-file>          # must be empty: never committed anywhere
git log --all -S"LTAI" --oneline                    # token formats never introduced
git log --all -S"BEGIN RSA PRIVATE KEY" --oneline
```
And check author/committer identities for anything public:
`git log --format='%ae | %ce' <REF> | sort -u` —
personal emails in *history* leak even if the tree is clean. Prefer a
fresh orphan release branch (`git archive`) authored with a noreply
address over deleting files from a full-history branch.

## Rules of engagement

- Audit the EXACT ref being pushed, in a clean checkout of it.
- Record the audit result (date + outcome) in the project's publishing
  rules — a baseline makes the next diff-audit fast.
- If a finding is a documentation placeholder, whitelist it by exact
  string in the recorded baseline; anything else is a stop.
- `.gitignore`/ignore-files are committed: they may name secrets, never
  contain them.

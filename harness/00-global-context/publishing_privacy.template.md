<!-- Template: publishing & privacy — tier 0. Written for the day anything
     leaves the machine. Edit points: branch names, identity notes; keep the
     audit block intact — it is the operational core. -->

# Publishing & Privacy Rules

> Read before anything from this repo leaves the machine or becomes public.

## The one rule

Only the {{public-release branch}} ever goes public. Dev history ({{main}},
feature branches) must never be pushed to a public repo, and the private
origin's visibility must never be flipped to public.

Why: dev history carries {{personal author emails, machine usernames in
docs/logs}}. The release branch was authored with a noreply identity and
curated to contain none of that — keeping that separation is the whole
defense. Prefer a separate orphan branch built via `git archive` over
deleting files: deletions miss things, archives can't leak what they
don't contain.

## Credentials

- {{secrets file}} is gitignored and must never be committed. Keys live at
  runtime only — environment or that file.
- A committed example file (`.{{name}}.example`) carries empty values only.
- Never paste real keys into docs, tests, fixtures, decks or articles.

## Personal information

- Commit identity for anything public: a noreply address.
- No real email addresses in docs; use `user@example.com`.
- No absolute home paths in any doc that could ship; use repo-relative paths.
- No LAN/internal IP addresses.
- {{Domain-specific placeholders}} — e.g. device addresses in docs use an
  obvious placeholder, never a real captured one.
- Evidence and run logs are dev-internal; never copy them into the public
  branch or articles.

## Pre-publish audit (run before every public push)

Secret token formats (must return nothing):

```
git grep -I -n -E "LTAI[0-9A-Za-z]+|AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9]{20,}|AIza[0-9A-Za-z_-]{20,}|sk-[A-Za-z0-9]{16,}|BEGIN [A-Z ]*PRIVATE KEY" {{branch}}
git grep -I -n -iE "(api[_-]?key|secret|token|password)[\"' ]*[:=][\"' ]*[A-Za-z0-9+/]{8,}" {{branch}}
```

Personal-info patterns (only documentation placeholders may match):

```
git grep -I -n -iE "{{username}}|C:[/\\]Users|/Users/" {{branch}}
git grep -I -n -E "\b(192\.168\.|10\.[0-9]+\.|172\.(1[6-9]|2[0-9]|3[01])\.)[0-9]+\.[0-9]+\b" {{branch}}
```

History checks on all refs (must return nothing):

```
git log --all --oneline -- {{secrets file}}
git log --all -S"LTAI" --oneline
git log --all -S"BEGIN RSA PRIVATE KEY" --oneline
```

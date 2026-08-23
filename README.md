<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/github-mark-dark.svg">
  <img src=".github/assets/github-mark.svg" alt="" width="52">
</picture>

# GitHub Tricks

**The GitHub you did not know you had.**

<br>

[![Tricks](https://img.shields.io/badge/Tricks-170+-181717?logo=github&logoColor=white)](#start-here)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](LICENSE)

<br>

[Start here](#start-here) &nbsp;·&nbsp;
[One-word swaps](#one-word-swaps) &nbsp;·&nbsp;
[URLs](#urls) &nbsp;·&nbsp;
[Keyboard](#keyboard) &nbsp;·&nbsp;
[Search](#search) &nbsp;·&nbsp;
[Repository](#repository) &nbsp;·&nbsp;
[Actions](#actions) &nbsp;·&nbsp;
[Security](#security) &nbsp;·&nbsp;
[Profile](#profile) &nbsp;·&nbsp;
[Dead tricks](#dead-tricks)

<br>

*Every code block has a copy button. Every **Try it** link is live, on a real repository.*

</div>

---

## Start here

Ten that surprise almost everyone. Click any one and watch it work.

<div align="center">

<img src=".github/social/ten-tricks.png" alt="The ten tricks, as a card: gitmcp.io, gitingest.com, diff by date, ?plain=1, ?w=1, .atom feeds, .keys, -linked:pr, symbol: search, and purging camo" width="860">

</div>

<br>

**1. Turn any repository into a server your AI can read.** Nothing to install.

```
https://gitmcp.io/OWNER/REPO
```

[Try it on cli/cli](https://gitmcp.io/cli/cli)

<br>

**2. Get a whole codebase as one text file**, sized for a prompt.

```
https://gitingest.com/OWNER/REPO
```

[Try it on cli/cli](https://gitingest.com/cli/cli)

<br>

**3. See what changed in January**, on a repository you have never cloned.

```
https://github.com/OWNER/REPO/compare/main@{2026-01-01}...main@{2026-02-01}
```

[Try it on cli/cli](https://github.com/cli/cli/compare/trunk@%7B2026-01-01%7D...trunk@%7B2026-02-01%7D)

> [!NOTE]
> Undocumented, and unlike local Git it needs no reflog: GitHub resolves the date server-side. Encode the braces as `%7B` and `%7D` in scripts.

<br>

**4. Link to one line of a README.** Line anchors do nothing without `?plain=1`.

```
https://github.com/OWNER/REPO/blob/SHA/README.md?plain=1#L14
```

[Try it on cli/cli](https://github.com/cli/cli/blob/trunk/README.md?plain=1#L14)

<br>

**5. Read a diff without the reindentation noise.**

```
https://github.com/OWNER/REPO/pull/123/files?w=1
```

[Try it on cli/cli](https://github.com/cli/cli/pull/9000/files?w=1)

<br>

**6. Subscribe to a single file.** RSS for one path, not the whole repository.

```
https://github.com/OWNER/REPO/commits/main/README.md.atom
```

[Try it on cli/cli](https://github.com/cli/cli/commits/trunk/README.md.atom)

<br>

**7. Take anyone's public SSH keys**, ready for `authorized_keys`.

```
https://github.com/USERNAME.keys
```

[Try it on Amey-Thakur](https://github.com/Amey-Thakur.keys)

<br>

**8. Find work nobody has claimed.** Open issues with no pull request attached.

```
is:issue is:open -linked:pr
```

[Try it on cli/cli](https://github.com/cli/cli/issues?q=is%3Aissue+is%3Aopen+-linked%3Apr)

<br>

**9. Search definitions only**, instead of four hundred call sites.

```
symbol:NewClient
```

[Try it](https://github.com/search?q=symbol%3ANewClient&type=code)

<br>

**10. Unstick a badge that will not refresh.** GitHub proxies every external image through camo and caches it.

```bash
curl -X PURGE https://camo.githubusercontent.com/HASH
```

> [!TIP]
> Right-click the stale badge and copy its image address to get the camo URL.

<br>

> [!IMPORTANT]
> `/stargazers` now returns 404 to the public, `github-readme-stats` is unmaintained, and `uithub.com` demands a login. [Everything else that quietly died](#dead-tricks).

<br>

## One-word swaps

Change one word in the address bar. Same repository, entirely different thing.
Each hostname below is a live link, pointed at `cli/cli`.

<div align="center">

`github.com/owner/repo` &nbsp;→&nbsp; `github`**`1s`**`.com/owner/repo`

</div>

<br>

<div align="center">

<img src=".github/social/diff.png" alt="The same repository URL shown as a diff: github.com removed, github1s.com, gitingest.com and gitmcp.io added" width="860">

</div>

<br>

| Swap to | You get | Catch |
|---|---|---|
| [`github1s.com`](https://github1s.com/cli/cli) | VS Code in the browser | Read-only. Private repos need a pasted token |
| [`github.dev`](https://github.dev/cli/cli) | The same editor, with write access | No terminal, no builds. Press <kbd>.</kbd> from any repo |
| [`gitingest.com`](https://gitingest.com/cli/cli) | The repository as one prompt-sized file | Silently skips files over 50 kB |
| [`gitmcp.io`](https://gitmcp.io/cli/cli) | An MCP server for your AI | `gitmcp.io/docs` floats to any repo. Scope it |
| [`deepwiki.com`](https://deepwiki.com/cli/cli) | A generated wiki, with its own MCP | Public repositories only |
| [`gitdiagram.com`](https://gitdiagram.com/cli/cli) | An architecture diagram | An LLM reading the file tree, not import analysis |
| [`gitreverse.com`](https://gitreverse.com/cli/cli) | A "build this from scratch" prompt | Reads the README and metadata only |
| [`forgithub.com`](https://forgithub.com/cli/cli) | The index of every swap tool | Unmaintained. Two of its entries are dead |

<br>

## URLs

### Diffs and patches

```
/pull/123.diff                      plain unified diff
/pull/123.patch                     mbox series, replays with git am
/pull/123/files?w=1                 hide whitespace-only changes
/commit/SHA?diff=split              side by side
/compare/v1.0.0...v2.0.0.patch      a range the web page refuses to render
/compare/main...OTHER_OWNER:main    across forks
/compare/SHA~5...SHA                a commit against five before it
```

[A patch series](https://github.com/cli/cli/pull/9000.patch) &nbsp;·&nbsp; [Split view](https://github.com/cli/cli/commit/HEAD?diff=split)

> [!WARNING]
> **`A...B` is not `A..B`.** Three dots diff from the merge base, which is what a pull request shows. Two dots diff the current tips, so the answer changes whenever the base branch moves. Say which one you used when you paste a compare link into a bug report.

<br>

### Files, lines and blame

```
/blob/REF/README.md?plain=1                 line numbers on rendered Markdown
/blob/REF/PATH#L10-L20                      highlight a range
/blame/REF/PATH                             blame, or press b
/find/BRANCH                                the file finder, or press t
raw.githubusercontent.com/O/R/SHA/PATH      raw, immutable, uncached
```

[Blame a file](https://github.com/cli/cli/blame/trunk/README.md) &nbsp;·&nbsp; [Open the file finder](https://github.com/cli/cli/find/trunk)

> [!CAUTION]
> A raw URL carrying a **branch** name is cached about five minutes, so a fresh push looks like it did nothing. Swap in the commit SHA and it is immutable and instant.

> [!NOTE]
> The file finder hides `.git`, `.hg`, `.sass-cache`, `.svn`, `build`, `dot_git`, `log`, `tmp` and `vendor`, so a file you know exists can look missing. Blame hides anything in `.git-blame-ignore-revs`; append `~` to the SHA in the URL to see past it.

<br>

### Downloads, feeds and identity

```
/archive/refs/heads/BRANCH.tar.gz     any branch as a tarball
/archive/refs/tags/TAG.zip            any tag, slashes included
/releases/latest/download/tool.zip    always the newest release asset
/commits/main/README.md.atom          RSS for one file
/releases.atom  /tags.atom  /discussions.atom
github.com/USERNAME.keys              public SSH keys
github.com/USERNAME.gpg               public PGP key
github.com/USERNAME.png?size=100      avatar, any size
```

[Releases feed](https://github.com/cli/cli/releases.atom) &nbsp;·&nbsp; [Latest release](https://github.com/cli/cli/releases/latest) &nbsp;·&nbsp; [An avatar](https://github.com/Amey-Thakur.png?size=100)

> [!NOTE]
> `.gpg` never 404s. An account with no key returns a well-formed but empty armored block, so check the body rather than the status code. Archive URLs redirect to `codeload.github.com`, which strict firewalls block.

<br>

### Two hidden repositories

```bash
git clone https://github.com/OWNER/REPO.wiki.git    # the wiki is its own repo
git fetch origin pull/123/head:pr-123               # any PR, including closed ones
```

> [!WARNING]
> A wiki is never in a clone and never in an archive download. Backing up the repository does not back up the wiki.

<br>

### Prefilled forms

```
/issues/new?title=Bug&body=Steps&labels=bug&assignees=octocat
/compare/main...branch?quick_pull=1&title=Fix&template=release.md
/releases/new?tag=v1.0.1&title=v1.0.1&prerelease=1
/commits/main?author=USER&since=2025-01-01&until=2025-01-08
```

> [!CAUTION]
> Permission is checked per parameter, and failure is all-or-nothing. One parameter you lack rights for turns the whole URL into a 404 rather than a partly filled form.

<br>

## Keyboard

<div align="center">

<img src=".github/social/keys.png" alt="Five keycaps: full stop for the editor, t for find file, y for permalink, b for blame, question mark for every key" width="760">

</div>

<br>

| Key | Does | Worth knowing |
|---|---|---|
| <kbd>?</kbd> | Shortcuts for the page you are on | The docs list only some. The dialog is the authority |
| <kbd>t</kbd> | Fuzzy file finder | Hides nine directories |
| <kbd>y</kbd> | Rewrites a branch URL to a permalink | No confirmation, no reload |
| <kbd>.</kbd> | Opens github.dev | <kbd>></kbd> opens it in a new tab |
| <kbd>b</kbd> | Blame | |
| <kbd>l</kbd> | Label filter | <kbd>Alt</kbd> + click a label to **exclude** it |
| <kbd>r</kbd> | Quotes the selected text in a reply | Appends, so repeats stack |
| <kbd>Alt</kbd> <kbd>↑</kbd> | Moves focus **into** a hovercard | The only keyboard route to one |
| <kbd>Ctrl</kbd> <kbd>.</kbd> | Saved replies, then <kbd>Ctrl</kbd> <kbd>N</kbd> | Max 100, personal, not shareable |
| <kbd>Ctrl</kbd> <kbd>G</kbd> | Suggestion block from the selected lines | Extra lines get deleted when applied |
| <kbd>g</kbd> then <kbd>c</kbd> <kbd>i</kbd> <kbd>p</kbd> <kbd>a</kbd> <kbd>s</kbd> | Code, Issues, PRs, Actions, Security | <kbd>g</kbd> <kbd>s</kbd> is Security, never Settings |
| <kbd>Shift</kbd> <kbd>T</kbd> | Timestamps in Actions logs | Off by default, so long waits are invisible |
| <kbd>e</kbd> | Archives the selection in Projects | **No confirmation.** Takes the whole selection |

**Two modifier clicks nobody documents.** <kbd>Alt</kbd> + click a file's caret collapses *every* diff in the pull request. <kbd>Alt</kbd> + click a resolved thread toggles *all* outdated threads.

> [!TIP]
> Turn the lot off at [github.com/settings/accessibility](https://github.com/settings/accessibility) by deselecting "Character keys". Modifier shortcuts keep working.

<br>

## Search

### Code

```
/sparse.*index/                regex, no look-around, escape every slash
/(?-i)True/                    case-sensitive, which is not the default
symbol:NewClient               definitions only, never call sites
path:/src/**/*.js              anchored glob
content:README.md              the text, not the filename
NOT is:fork                    drop forks
```

[Run a symbol search](https://github.com/search?q=symbol%3ANewClient&type=code)

> [!IMPORTANT]
> An absent result never proves the code is absent. Code search skips every branch except the default one, files over 350 KiB, any file with a line over 4,096 bytes, and non-UTF-8 files. Results stop at 100, and quoting a `path:` value silently disables globbing.

<br>

### Issues and pull requests

```
is:issue is:open -linked:pr           nobody is working on these
reason:completed                      closed as done
reason:"not planned"                  closed as abandoned
user-review-requested:@me             asked of you, not of your team
no:label no:assignee no:milestone     untriaged
type:"Bug"  field.Priority:High       issue types and fields
archived:false                        skip archived repositories
interactions:>2000                    reactions plus comments
sort:reactions-+1                     ranked by thumbs up
```

[See unclaimed issues](https://github.com/cli/cli/issues?q=is%3Aissue+is%3Aopen+-linked%3Apr)

> [!WARNING]
> `label:a,b` means **or**. `label:a label:b` means **and**. Nothing on screen tells you which you built. Closing an issue as *duplicate* matches neither `reason:completed` nor `reason:"not planned"`, so two-bucket queries lose them silently.

<br>

### Repositories and commits

```
topics:>3                        by topic count, not topic: name
size:1000..2000                  kilobytes, not megabytes
org:NAME props.production:true   organisation custom properties
parent:SHA                       a commit's children
merge:true                       merge commits only
saved:                           type this in the search bar for saved queries
```

> [!NOTE]
> Every search API result set stops at 1,000. When a query times out the API returns `incomplete_results: true` **with a 200 status**, so a partial answer looks complete.

<br>

## Repository

### CODEOWNERS, and the four rules that quietly break it

```
/docs/     @team     recursive
docs/*     @team     ONE level deep, and nothing warns you
```

> [!CAUTION]
> **Last match wins, not most specific.** A `* @org/everyone` catch-all at the bottom, which reads like a sensible fallback, disables every rule above it.
>
> Invalid lines are skipped silently. A team without write access is dropped silently. The file is read from the **base** branch, so a pull request that adds itself as an owner cannot apply that rule to itself.

<br>

### Merging

> [!CAUTION]
> **A required check skipped by path filtering blocks the pull request forever.** Skipped is not passed, and no timeout converts one into the other. Fix it with a second workflow of the same job name, on the complementary paths, that exits 0.

A merge queue stalls silently unless every required workflow listens for it:

```yaml
on:
  merge_group:
```

`Fixes owner/repo#100` closes an issue in another repository, but is ignored unless the pull request targets the **default** branch. Open as a draft to defer code-owner pings until you are ready.

<br>

### Rules, releases and Pages

- **Rulesets stack, they do not replace.** Several rulesets plus any legacy branch protection all apply at once and the most restrictive wins, which is why half-finished migrations block people for reasons nobody can find.
- **Push rulesets reach into forks.** Restricting paths, extensions or file size is the only rule family that applies to repositories you do not own.
- **`.github/release.yml`** shapes autogenerated release notes. `*` is the catch-all, and categories are evaluated in order.
- **Immutable releases are one-way.** Delete one and the tag name can never be reused.
- **Actions-based Pages ignores `CNAME` entirely.** Set the custom domain in Settings; a generator that writes `CNAME` into the build does nothing.
- **`.nojekyll`, or lose your build.** Without it Jekyll deletes every directory starting with an underscore, which means `_next`, `_app` and `_assets`.
- **Secret gists are unlisted, not private,** and public can never go back to secret.

<br>

## Actions

### The failures that give no error

| Symptom | Cause | Fix |
|---|---|---|
| A workflow never fires after another one pushed | `GITHUB_TOKEN` actions do not trigger workflows | Use an App token or a PAT |
| `if:` fires on `"false"` | `github.event.inputs` turns booleans into strings | Use the `inputs` context |
| `actions/checkout` suddenly 403s | Naming one permission sets every other to `none` | List `contents: read` as well |
| The cron stopped months ago | Public repos disable schedules after 60 days idle | Push anything, or re-enable in Actions |
| The cache never warms on a pull request | Untrusted triggers get a read-only cache token | No opt-out. Warm it on the default branch |
| Every new branch builds slowly | Caches do not cross sibling branches | Build the cache on the default branch |
| A matrix job's output vanished | Every leg writes the same key, last wins | Use artifacts instead |
| An output disappeared with no error | It resembled a secret and was masked | Look for the skip warning in the log |
| `workflow_run` linted the wrong code | It runs on the default branch | Check out the triggering SHA explicitly |

<br>

### Worth having

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice          # real form controls on manual runs
        options: [staging, production]

concurrency:
  group: deploy
  queue: max                  # queue up to 100 instead of cancelling

steps:
  - run: make assets
    background: true          # parallel steps in one job, added June 2026
  - wait
  - run: echo "### Report" >> "$GITHUB_STEP_SUMMARY"
```

Set a repository variable `ACTIONS_STEP_DEBUG` to `true` for debug logs without touching the YAML. Run workflows locally with `act`, which ignores `concurrency` and `permissions`. Audit them with `zizmor`, and type-check expressions with `actionlint`.

<br>

### The two that hand over your repository

> [!CAUTION]
> **`pull_request_target` plus a checkout of the pull request head.** The job runs with your secrets and a write token, then executes a stranger's `postinstall` script. Nothing in the diff has to look malicious.
>
> **`${{ github.event.* }}` inside `run:`.** Substitution happens before the shell ever sees the line, so quoting does not save you.

```yaml
- env:
    TITLE: ${{ github.event.pull_request.title }}   # safe: data, not script
  run: echo "$TITLE"
```

Pin every third-party action to a full 40-character SHA. Short SHAs are rejected, and Dependabot keeps both the pin and its version comment current.

<br>

## Security

Defaults that are not what people assume.

| You probably think | Actually |
|---|---|
| Bypassing push protection raises an alert | Not on public repos, unless repository-level protection is also on |
| CodeQL keeps scanning | It stops after six months with no pushes |
| Dependabot reads your Actions secrets | A different store entirely, under Settings → Dependabot |
| Dependabot watches `.github/workflows` | Only with `directory: "/"`. The intuitive path finds nothing |
| You see all your alerts | A preset rule auto-dismisses npm dev-dependency alerts on public repos |
| A leaked key appears in your Security tab | Partner-pattern secrets go straight to the provider instead |
| `cooldown` delays everything | Security fixes ignore it, and ignore the open-PR limit |

> [!WARNING]
> OIDC subject claims changed format for repositories created after **15 July 2026**. A cloud trust policy copied from an older repository silently never matches.

Free and off by default: **private vulnerability reporting**, the only private channel a researcher has. Free and worth turning on: **vigilant mode**, which flags forged unsigned commits as Unverified. Free on public repositories: **secret scanning** across history, branches, issues, wikis and gists.

```bash
gh attestation verify ./app -R OWNER/REPO \
  --signer-workflow OWNER/REPO/.github/workflows/release.yml
```

> [!NOTE]
> Without `--signer-workflow` or `--signer-repo`, `gh attestation verify` accepts almost any attestation from anywhere. The bare command is not the check you think it is.

<br>

## Profile

```
USERNAME/USERNAME            a public repo named exactly your username
.github/profile/README.md    an organisation's profile
.github/                     one public repo, defaults for every repo you own
```

> [!CAUTION]
> **`.github/README.md` outranks your root README.** The lookup order is `.github/`, then the root, then `docs/`. A contributor note in the wrong place replaces your front page, and nothing tells you.

**Why your commit is missing from the graph.** The author email must be linked to your account, the repository must not be a fork, and the commit must be on the default branch or on `gh-pages`, which is the one special case nobody knows.

**Why your stats card broke.** `github-readme-stats` is unmaintained and its shared instance returns 503. Generate cards inside your own Actions run instead. Badges go blank because shields.io runs on donated rate limit, and you can lend yours at [img.shields.io/github-auth](https://img.shields.io/github-auth).

**Why your snake stopped moving.** Sixty days of repository inactivity disables scheduled workflows.

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="dark.png">
  <img src="light.png" alt="">
</picture>
```

> [!TIP]
> The "only nine tags are filtered" line everyone quotes is the GFM **spec**, not GitHub. GitHub adds a stricter allowlist on top. `<script>` and `<iframe>` are escaped, so you notice them, but `<video>`, `<form>`, `<input>`, `<button>` and inline `<svg>` are **deleted with no trace**. `<img width>`, `<picture>`, `<details>` and `<kbd>` all survive, and this README uses every one. Any `style` or `class` attribute is stripped, and every `id` you write is silently prefixed with `user-content-`.

Social preview images are 1280 × 640 and must be under 1 MB, and the upload refuses rather than resizes. Topics cap at 20, and the API `PUT` replaces the whole set rather than adding to it.

<br>

## Dead tricks

Every list still repeats these. None of them work.

<div align="center">

<img src=".github/social/dead-tricks.png" alt="Three dead tricks struck through: /stargazers 404s, github-readme-stats returns 503, uithub.com requires a login" width="860">

</div>

<br>

| The tip you have seen | Reality, checked 22 August 2026 |
|---|---|
| `/stargazers` and `/watchers` | 404 to the public since 30 June 2026, REST endpoints included |
| star-history chart embeds | An SVG reading "GitHub restricted access to star data" |
| `github-readme-stats.vercel.app` | Unmaintained. The shared instance returns 503 |
| `uithub.com` | 401 Unauthorized, even for a public repository |
| `openrepowiki.xyz` | A GoDaddy for-sale page |
| `talktogithub.com` | Ad-parked |
| "Octotree is free and open source" | The shipping extension is proprietary |
| Gitpod | Sunset 15 October 2025. The company is now Ona |
| <kbd>x</kbd> selects issues | Never existed |
| `#gh-dark-mode-only` | Deprecated. Use `<picture>` |
| `directory: ".github/workflows"` | Finds nothing. Use `"/"` |
| Tasklist blocks | Retired |
| `actions/attest-build-provenance` | Now a wrapper. Call `actions/attest` |
| The dispatch API returns 204 | Returns the run id, since 2026-03-10 |

<br>

## Share it

Cards sized for every platform live in [`.github/social`](.github/social): `address-bar`, `diff`, `keys`, `dead-tricks` and `ten-tricks` at 1280 &times; 640, plus `square` at 1080 &times; 1080 for WhatsApp and Instagram.

<br>

## Contributing

Found a mistake, or a trick that belongs here? [Open an issue](https://github.com/Amey-Thakur/GITHUB-TRICKS/issues/new). A link to the source gets it in fastest.

Every entry was read against its primary source on 22 August 2026. Undocumented tricks are labelled, because they can change without notice.

<br>

<div align="center">

Git itself, command by command, is in [**GIT-GUIDE**](https://github.com/Amey-Thakur/GIT-GUIDE).

<br>

**[Amey Thakur](https://github.com/Amey-Thakur)** &nbsp;·&nbsp; [MIT](LICENSE)

</div>

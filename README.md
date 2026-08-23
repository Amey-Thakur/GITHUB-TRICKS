<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/github-mark-dark.svg">
  <img src=".github/assets/github-mark.svg" alt="" width="52">
</picture>

# GitHub Tricks

**The GitHub you did not know you had.**

<br>

[![Tricks](https://img.shields.io/badge/Tricks-170+-181717?logo=github&logoColor=white)](#start-here)
[![Verified](https://img.shields.io/badge/Every_entry-checked_at_source-2EA043?logo=github&logoColor=white)](#how-this-was-checked)
[![Updated](https://img.shields.io/badge/Updated-22_Aug_2026-8250DF?logo=github&logoColor=white)](#how-this-was-checked)
[![Curated by](https://img.shields.io/badge/Curated_by-Amey_Thakur-0969DA?logo=github&logoColor=white)](https://github.com/Amey-Thakur)
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

</div>

---

## Start here

The ten that surprise almost everyone.

| | You get | Type this |
|---|---|---|
| **Repo → MCP server** | Any public repo as a server your AI can read, nothing installed | `gitmcp.io/OWNER/REPO` |
| **Repo → one prompt** | The whole codebase as a single text file | `gitingest.com/OWNER/REPO` |
| **What changed in January** | A diff by date, on a repo you never cloned | `/compare/main@{2026-01-01}...main@{2026-02-01}` |
| **Link one line of a README** | Line anchors that actually work on Markdown | `/blob/SHA/README.md?plain=1#L14` |
| **Kill the whitespace noise** | A diff without reindentation | `/pull/123/files?w=1` |
| **Watch one file** | An RSS feed for a single path | `/commits/main/README.md.atom` |
| **Someone's SSH keys** | Public keys, ready for `authorized_keys` | `github.com/USERNAME.keys` |
| **Find unclaimed work** | Open issues nobody has a PR for | `is:issue is:open -linked:pr` |
| **Search definitions only** | Where it is defined, not the 400 call sites | `symbol:deleteRows` |
| **Unstick a stale badge** | Force GitHub's image proxy to refetch | `curl -X PURGE https://camo.githubusercontent.com/HASH` |

> [!NOTE]
> `/stargazers` now 404s, `github-readme-stats` is unmaintained, `uithub.com` wants a login. [Everything else that quietly died](#dead-tricks).

<br>

## One-word swaps

Change one word in the address bar. Same repo, different thing.

<div align="center">

`github.com/OWNER/REPO` → `github`**`1s`**`.com/OWNER/REPO`

</div>

<br>

| Swap to | You get | Catch |
|---|---|---|
| `github1s.com` | VS Code in the browser, read-only | Private repos need a token pasted into the status bar |
| `github.dev` &nbsp;or press <kbd>.</kbd> | GitHub's editor, with write access | No compute: no terminal, no builds |
| `gitingest.com` | The repo as one text file for a prompt | Skips files over 50 kB by default, silently |
| `gitmcp.io` | An MCP server for your AI | `gitmcp.io/docs` is a floating endpoint that can point anywhere. Scope it |
| `deepwiki.com` | A generated wiki, plus its own MCP | Public repos only; private needs `mcp.devin.ai` and a token |
| `gitdiagram.com` | An architecture diagram | An LLM reading the file tree, not real import analysis |
| `gitreverse.com` | A "build this" prompt | Reads metadata and the README only |
| `gitpodcast.com` | A spoken walkthrough | In-depth version needs sign-in |
| `github.gg` | Diagrams and a repo scorecard | Three free reviews per install |
| `pr.new/github.com/…` | StackBlitz Codeflow | Still beta |
| `forgithub.com` | The index of every swap tool | Not checked for rot; still lists two dead sites |

<br>

## URLs

<details open>
<summary><b>Diffs and patches</b></summary>

<br>

| You want | Type this | Catch |
|---|---|---|
| A PR as a unified diff | `/pull/N.diff` | 302s to another host, `curl` needs `-L` |
| A PR as an applyable patch series | `/pull/N.patch` | One mbox message per commit, replays with `git am` |
| A diff the web page refuses to render | `/compare/A...B.patch` | No size cap, a tag range can be 74 messages |
| A diff by date | `/compare/main@{2026-01-01}...main@{2026-02-01}` | Undocumented. Encode braces as `%7B` in scripts |
| A diff across forks | `/compare/main...OTHER_OWNER:main` | If a branch and tag share a name, the branch wins |
| The last five commits' worth | `/compare/SHA~5...SHA` | |
| No whitespace noise | `?w=1` on any diff URL | Undocumented since 2011. `.diff` and `.patch` ignore it |
| Side-by-side | `?diff=split` (or `unified`) | Combines: `?diff=split&w=1` |

**`A...B` is not `A..B`.** Three dots diff from the merge base, which is what a PR shows. Two dots diff the current tips, so the result changes whenever the base branch moves.

</details>

<details open>
<summary><b>Files, lines and blame</b></summary>

<br>

| You want | Type this | Catch |
|---|---|---|
| Line numbers on a rendered README | `?plain=1` | Must come **before** the `#L14` fragment |
| A permalink to lines 10–20 | `#L10-L20`, or press <kbd>y</kbd> | <kbd>y</kbd> gives no visual confirmation |
| A permalink that renders as a snippet | Copy permalink from the kebab menu | Only renders inside the repo it came from |
| Raw file, always fresh | `raw.githubusercontent.com/…/SHA/PATH` | Branch URLs are CDN-cached about five minutes |
| A gist raw URL that never changes | `gist.githubusercontent.com/U/ID/raw/REVISION_SHA/FILE` | Without the SHA it always serves latest |
| Blame | swap `/blob/` for `/blame/`, or <kbd>b</kbd> | `.git-blame-ignore-revs` hides commits here but not locally |
| Blame, ignoring the ignore file | append `~` to the SHA in the URL | |
| The file finder | `/find/BRANCH`, or <kbd>t</kbd> | Hides `vendor`, `build`, `log`, `tmp` and five more |

</details>

<details open>
<summary><b>Downloads, feeds and identity</b></summary>

<br>

| You want | Type this | Catch |
|---|---|---|
| A tarball of any ref | `/archive/refs/heads/BRANCH.tar.gz` | 302s to `codeload.github.com`, often firewalled |
| A tag with slashes in it | `/archive/refs/tags/cli/v2.12.0.zip` | |
| The newest release asset, forever | `/releases/latest/download/tool.zip` | Filename must stay identical across releases |
| RSS for a repo, branch or one file | `/commits/main/README.md.atom` | Every feed shares a title, rename them in your reader |
| RSS for releases, tags, discussions | `/releases.atom`, `/tags.atom`, `/discussions.atom` | |
| Anyone's SSH keys | `github.com/USERNAME.keys` | |
| Anyone's PGP key | `github.com/USERNAME.gpg` | Returns an empty block, not a 404, if none uploaded |
| Anyone's avatar at any size | `github.com/USERNAME.png?size=100` | Parameter is `size`, not `s` |
| Every PR ref, including closed | `/OWNER/REPO.git/info/refs?service=git-upload-pack` | pkt-line binary, not JSON |
| The wiki as a git repo | `git clone …/OWNER/REPO.wiki.git` | Separate repo. Never in a clone, never in an archive |

</details>

<details open>
<summary><b>Prefilled forms</b></summary>

<br>

| You want | Type this |
|---|---|
| A prefilled issue | `/issues/new?title=Bug&body=Details&labels=bug&assignees=octocat` |
| A prefilled PR | `/compare/main...branch?quick_pull=1&title=Fix&template=release.md` |
| A prefilled release | `/releases/new?tag=v1.0.1&title=v1.0.1&prerelease=1` |
| Commits by author and date | `/commits/main?author=USER&since=2025-01-01&until=2025-01-08` |

> [!WARNING]
> Permission is checked per parameter and failure is all-or-nothing. One parameter you lack rights for turns the whole URL into a 404, not a partial form.

</details>

<br>

## Keyboard

| Key | Does | Catch |
|---|---|---|
| <kbd>?</kbd> | Shortcuts for *this* page | The docs list only some; the dialog is the authority |
| <kbd>t</kbd> | Fuzzy file finder | Hides nine directories |
| <kbd>y</kbd> | Branch URL → commit permalink | No confirmation, page does not reload |
| <kbd>.</kbd> | Opens github.dev | <kbd>></kbd> for a new tab |
| <kbd>b</kbd> | Blame | |
| <kbd>l</kbd> | Label filter | <kbd>Alt</kbd>+click a label to **exclude** it |
| <kbd>r</kbd> | Quote selected text in a reply | Appends, so repeats stack |
| <kbd>Alt</kbd>+<kbd>↑</kbd> | Move focus **into** a hovercard | The only keyboard route to hovercard content |
| <kbd>Ctrl</kbd>+<kbd>.</kbd> then <kbd>Ctrl</kbd>+<kbd>N</kbd> | Insert saved reply N | Max 100, personal not shared |
| <kbd>Ctrl</kbd>+<kbd>G</kbd> | Suggestion block from selected lines | Extra lines in the block get deleted on apply |
| <kbd>g</kbd> <kbd>c</kbd> / <kbd>i</kbd> / <kbd>p</kbd> / <kbd>a</kbd> / <kbd>s</kbd> | Code, Issues, PRs, Actions, Security | <kbd>g</kbd> <kbd>s</kbd> is Security, not Settings |
| <kbd>Alt</kbd>+click a file caret | Collapse **every** diff | Announced 2018, never documented |
| <kbd>Alt</kbd>+click a resolved thread | Toggle all outdated threads | |
| <kbd>Shift</kbd>+<kbd>T</kbd> in Actions logs | Timestamps | Off by default, so waits are invisible |
| <kbd>e</kbd> in Projects | Archive selection | **No confirmation**, acts on the whole selection |
| <kbd>Ctrl</kbd>+<kbd>K</kbd> | Command palette | Off by default; enable in Feature preview |

**Turn it all off** at `/settings/accessibility` → deselect "Character keys". Modifier shortcuts keep working.

<br>

## Search

<details open>
<summary><b>Code</b></summary>

<br>

| You want | Type this | Catch |
|---|---|---|
| Regex | `/sparse.*index/` | No look-around. Escape every `/` |
| Case-sensitive | `/(?-i)True/` | Search is case-insensitive by default, silently |
| Definitions only | `symbol:deleteRows` | Never matches call sites |
| A path glob | `path:/src/**/*.js` | Quoting **disables** globbing, no warning |
| Text, not filenames | `content:README.md` | The fix for drowning in path hits |
| Excluding forks | `NOT is:fork` | Forks index only if they outstar the parent |
| Across a whole enterprise | `enterprise:octocorp` | Enterprise Cloud only |

> [!IMPORTANT]
> An absent result never proves the code is absent. Code search skips files over 350 KiB, any file with a line over 4,096 bytes, non-UTF-8 files, and every branch except the default one. Results cap at 100.

</details>

<details open>
<summary><b>Issues and pull requests</b></summary>

<br>

| You want | Type this | Catch |
|---|---|---|
| Issues nobody is working on | `is:issue is:open -linked:pr` | Only closing references count |
| Closed as done vs abandoned | `reason:completed` / `reason:"not planned"` | A third reason, duplicate, matches neither |
| Reviews actually assigned to you | `user-review-requested:@me` | `review-requested:` clears once you review |
| Untriaged | `no:label no:assignee no:milestone` | These cannot be negated with `-` |
| By issue type or field | `type:"Bug"`, `field.Priority:High` | Newest qualifiers, missing from every other list |
| Popular threads | `interactions:>2000` | Sums reactions **and** comments |
| Ranked by 👍 | `sort:reactions-+1` | 👎 is `sort:reactions--1`, two hyphens |
| Anything assigned | `assignee:*` | One repository at a time only |
| Skipping archived repos | `archived:false` | The filter every org dashboard forgets |
| Boolean logic | `(type:"Bug" AND assignee:me) OR label:urgent` | Five levels of nesting, hard ceiling |

`label:a,b` is OR. `label:a label:b` is AND. Nothing on screen tells you which you built.

</details>

<details>
<summary><b>Repositories, commits and the API</b></summary>

<br>

| You want | Type this |
|---|---|
| By topic count | `topics:>3` (not `topic:`) |
| By size, in kilobytes | `size:1000..2000` |
| By org custom property | `org:NAME props.production:true` |
| A commit's children | `parent:SHA` |
| Merge commits only | `merge:true` |
| Saved queries | type `saved:` in the search bar |
| Semantic issue search | REST `/search/issues?search_type=semantic` |

Every search API result set stops at 1,000. When a query times out the API returns `incomplete_results: true` **with a 200**, so partial answers look complete.

</details>

<br>

## Repository

<details open>
<summary><b>CODEOWNERS, the four rules that break it</b></summary>

<br>

| Rule | Consequence |
|---|---|
| `docs/*` is one level deep, `/docs/` is recursive | Nested files silently go to the wrong owner |
| **Last** match wins, not the most specific | A `*` catch-all at the bottom disables the whole file |
| Invalid lines are skipped silently | And a team without write access is dropped |
| Read from the **base** branch | A PR that adds itself as owner cannot self-apply |

</details>

<details open>
<summary><b>Pull requests, merging, rules</b></summary>

<br>

| You want | Do this | Catch |
|---|---|---|
| Several PR templates | `.github/PULL_REQUEST_TEMPLATE/` + `?template=x.md` | No chooser UI, unlike issues |
| Close an issue in another repo | `Fixes owner/repo#100` | Ignored unless the PR targets the default branch |
| Quiet work in progress | Open as draft | Defers code-owner pings until ready |
| A merge queue that works | `on: merge_group:` in every required workflow | Without it the queue waits forever, silently |
| Rules that stack | Rulesets + old branch protection both apply | Most restrictive wins; half-migrations block people |
| Rules across the fork network | Push rulesets | The only rules that reach repos you do not own |
| Release notes, shaped | `.github/release.yml` | `*` is a catch-all, categories run in order |

> [!CAUTION]
> A required check on a path-filtered workflow blocks the PR **forever**. Skipped is not passed, and there is no timeout. Fix it with a second workflow of the same job name on the complementary paths that exits 0.

</details>

<details>
<summary><b>Pages, wikis, gists, releases</b></summary>

<br>

| Thing | What nobody tells you |
|---|---|
| Actions-based Pages | The `CNAME` file is ignored entirely. Set the domain in Settings |
| `.nojekyll` | Without it Jekyll deletes every `_next`, `_app`, `_assets` directory |
| Secret gists | Unlisted, not private. Public → secret is impossible |
| Immutable releases | Delete one and the tag name can never be reused |
| Template repos | Unrelated histories, so no PRs between template and copy |
| Deleting a private repo | Takes its private forks with it |
| Q&A answers | Only Q&A-format categories accept them. 25 categories max |

</details>

<br>

## Actions

<details open>
<summary><b>The silent failures</b></summary>

<br>

| Symptom | Cause | Fix |
|---|---|---|
| A workflow never fires after another one pushed | `GITHUB_TOKEN` actions do not trigger workflows | Use an App token or PAT |
| `if:` fires on `"false"` | `github.event.inputs` stringifies booleans | Use the `inputs` context |
| `actions/checkout` 403s | Naming one permission sets all others to `none` | List `contents: read` too |
| Cron stopped months ago | Public repos disable schedules after 60 days idle | Push anything, or re-enable in Actions |
| Cache never warms on a PR | Untrusted triggers get a read-only cache token | No opt-out. Warm on the default branch |
| First build on every branch is slow | Caches do not cross sibling branches | Build the cache on the default branch |
| A matrix job's output vanished | All legs write the same key, last wins | Use artifacts |
| An output silently disappeared | It looked like a secret and got masked | Check the run log for the skip warning |
| `workflow_run` linted the wrong code | It runs on the default branch | Check out the triggering SHA explicitly |

</details>

<details open>
<summary><b>Worth knowing</b></summary>

<br>

| You want | Do this |
|---|---|
| Real form controls on manual runs | `workflow_dispatch` with `type: choice`, `boolean`, `environment` |
| Parallel steps in one job | `background: true`, then `wait` (added June 2026) |
| Queue instead of cancel | `concurrency: {queue: max}` |
| A Markdown report on the run page | `echo "..." >> "$GITHUB_STEP_SUMMARY"` |
| Multiline step output | Heredoc with a random delimiter into `$GITHUB_OUTPUT` |
| An artifact from another run | `download-artifact` with `run-id` **and** `github-token` |
| Debug logs without editing YAML | Repo variable `ACTIONS_STEP_DEBUG=true` |
| Skip a run | `[skip ci]` in the commit message |
| To run it locally | `act`, which ignores `concurrency` and `permissions` |
| To audit for injection | `zizmor`, and `actionlint` for expression types |

</details>

<details open>
<summary><b>The two that get repositories taken over</b></summary>

<br>

> [!CAUTION]
> **`pull_request_target` + checking out the PR head.** The job runs with your secrets and a write token, then executes a stranger's `postinstall` script. Nothing has to look malicious.
>
> **`${{ github.event.* }}` inside `run:`.** Substitution happens before the shell sees the line, so quoting does not save you. Pass it through `env:` and reference the variable.

Pin every third-party action to a full 40-character SHA. Short SHAs are rejected; Dependabot keeps the pin and the version comment in sync.

</details>

<br>

## Security

| Default that surprises people | Reality |
|---|---|
| Push protection alerts you when bypassed | Not on public repos, unless repo-level protection is also on |
| CodeQL keeps scanning | It stops after six months with no pushes |
| Dependabot reads your Actions secrets | Different store entirely, under Settings → Dependabot |
| Dependabot watches `.github/workflows` | Only if `directory: "/"`. The intuitive path finds nothing |
| Your alerts are all shown | A preset rule auto-dismisses npm dev-dependency alerts on public repos |
| A leaked key shows in Security | Partner-pattern secrets go straight to the provider, never your tab |
| Groups cover security updates | Only with `applies-to: security-updates` |
| `cooldown` delays everything | Security fixes ignore it |

| You want | Do this |
|---|---|
| A CVE | Draft a security advisory, click Request CVE. About 72 hours |
| A private report channel | Enable private vulnerability reporting. **Off by default** |
| One policy for every repo | `SECURITY.md` in a **public** `.github` repo |
| Forged commits flagged | Vigilant mode, under SSH and GPG keys |
| A real provenance check | `gh attestation verify --signer-workflow …`, not the bare command |
| An SBOM | `GET /repos/O/R/dependency-graph/sbom` |
| To block bad dependencies in review | `dependency-review-action` |

> [!WARNING]
> OIDC subject claims changed format for repositories created after 15 July 2026. A cloud trust policy copied from an older repo silently never matches.

<br>

## Profile

<details open>
<summary><b>The README itself</b></summary>

<br>

| You want | Do this | Catch |
|---|---|---|
| A profile README | Public repo named exactly your username | Break any condition and it vanishes silently |
| An org profile | `.github` repo → `profile/README.md` | Personal accounts cannot use this path |
| Defaults for every repo you own | A **public** `.github` repo | Private ones are ignored |
| Dark and light images | `<picture>` + `prefers-color-scheme` | `#gh-dark-mode-only` is deprecated |
| A stale badge refreshed | `curl -X PURGE` the camo URL | Use sparingly |
| Sizing an image | `<img width="500">` | Only nine HTML tags are filtered, the rest work |
| A collapsible section | `<details><summary>` | Needs a blank line before Markdown inside |

**`.github/README.md` outranks your root README.** Search order is `.github/`, root, `docs/`. A contributor note in the wrong place replaces your front page.

</details>

<details open>
<summary><b>Contributions, cards and badges</b></summary>

<br>

| Question | Answer |
|---|---|
| Why is my commit not on the graph? | Email must be linked, repo must not be a fork, branch must be default **or `gh-pages`** |
| Where is the contribution calendar API? | GraphQL only. No REST endpoint exists |
| Why did my stats card break? | `github-readme-stats` is unmaintained; the shared instance 503s |
| Why do badges go blank at random? | shields.io runs on donated rate limit. Lend yours at `/github-auth` |
| Why did my snake stop moving? | 60 days of repo inactivity disables scheduled workflows |
| A badge from my own JSON? | `img.shields.io/endpoint?url=…` with `schemaVersion`, `label`, `message` |
| Social preview size? | 1280 × 640, under 1 MB. It refuses rather than resizes |
| Topic limits? | 20 max, 50 chars. The API `PUT` replaces the **whole** set |
| Wrong language bar? | `.gitattributes` with `linguist-vendored`, `linguist-generated` |

</details>

<br>

## Dead tricks

Stop recommending these.

| The tip you have seen | Reality on 22 August 2026 |
|---|---|
| `/stargazers`, `/watchers` | 404 to the public since 30 June 2026. The REST endpoints too |
| star-history chart embeds | Returns an SVG saying the data is restricted |
| `github-readme-stats.vercel.app` | Unmaintained, shared instance returning 503 |
| `uithub.com` | Sign-in wall, even for public repos |
| `openrepowiki.xyz` | A GoDaddy for-sale page |
| `talktogithub.com` | Ad-parked |
| "Octotree is free and open source" | The shipping extension is proprietary |
| Gitpod | Sunset 15 October 2025, now Ona |
| <kbd>x</kbd> to select issues | Never existed |
| `#gh-dark-mode-only` | Deprecated, use `<picture>` |
| `directory: ".github/workflows"` | Finds nothing, use `"/"` |
| Tasklist blocks | Retired |
| `actions/attest-build-provenance` | Now a wrapper, call `actions/attest` |
| Dispatch API returns 204 | Returns the run id since 2026-03-10 |

<br>

## Contributing

Found a mistake, or a trick that belongs here? [Open an issue](https://github.com/Amey-Thakur/GITHUB-TRICKS/issues/new). A link to the source gets it in fastest.

Every entry here was read against its primary source on 22 August 2026. Undocumented tricks are labelled, because they can change without notice.

<br>

<div align="center">

Git itself, command by command, is in [**GIT-GUIDE**](https://github.com/Amey-Thakur/GIT-GUIDE).

<br>

**[Amey Thakur](https://github.com/Amey-Thakur)** &nbsp;·&nbsp; [MIT](LICENSE)

</div>

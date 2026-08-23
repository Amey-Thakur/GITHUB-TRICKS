<div align="center">

# GitHub Tricks

**Every GitHub feature, URL trick, keyboard shortcut, search qualifier, Actions pattern and third-party tool worth knowing. Each one checked against the source before it was written down.**

<br>

[![Status](https://img.shields.io/badge/Status-Active-2EA043)](#github-tricks)
[![Focus](https://img.shields.io/badge/Focus-GitHub_power_use-BF3989)](#github-tricks)
[![Curated by](https://img.shields.io/badge/Curated_by-Amey_Thakur-0969DA)](https://github.com/Amey-Thakur)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](LICENSE)

<br>

[URL tricks](#1-url-tricks) &nbsp;·&nbsp; [One-word swaps](#2-one-word-swaps) &nbsp;·&nbsp; [Keyboard](#3-keyboard-and-interface) &nbsp;·&nbsp; [Search](#4-search-syntax) &nbsp;·&nbsp; [Repository](#5-repository-and-platform) &nbsp;·&nbsp; [Actions](#6-actions-and-automation) &nbsp;·&nbsp; [Security](#7-security-and-supply-chain) &nbsp;·&nbsp; [Profile](#8-profile-readme-and-badges) &nbsp;·&nbsp; [Dead tools](#9-dead-and-changed)

</div>

---

## Why this exists

Most GitHub tip lists are copied from one another, and the copies carry the same errors forward. Three of the most repeated "tricks" no longer work: the `/stargazers` page returns 404 to the public, `github-readme-stats` is unmaintained and its shared instance returns 503, and `uithub.com` now demands a sign-in before showing a public repository. Several more never worked as described.

Every entry below was verified on 22 August 2026 against GitHub's documentation, GitHub's changelog, the tool's own README, or a live request. Where a trick is undocumented and could change, it says so. Where a tool is dead, it is listed as dead, so you do not have to discover that yourself.

Each entry gives the trick, the exact form to use, and the thing that goes wrong when you use it. The gotcha is usually the part nobody else writes down.

<br>

## 1. URL tricks

Things you can do by editing the address bar on github.com.

<br>

### Diffs and patches

**Append `.diff` or `.patch` to a pull request or commit.**
`https://github.com/OWNER/REPO/pull/NUMBER.diff` returns one unified diff. `.patch` returns a git-format-patch mbox, one message per commit, that `git am` replays as individual commits. Works on `/commit/SHA` too.
Pull request patches 302 to `patch-diff.githubusercontent.com`, so `curl` needs `-L` and a proxy that only allows github.com breaks. Commit patches stay on github.com.

**Append `.diff` or `.patch` to a compare URL.**
`https://github.com/OWNER/REPO/compare/v2.60.0...v2.61.0.patch` returns the complete series between two refs, including ranges the web page refuses with "This comparison is taking too long to generate". A tag-to-tag range can hand you a 74-message mbox in one response.

**Compare a branch against itself at a past date.**
`https://github.com/OWNER/REPO/compare/main@{2026-01-01}...main@{2026-02-01}` answers "what changed in January" without cloning. Relative form: `main@{2weeks}...main`. In scripts, encode the braces: `main@%7B2026-01-01%7D`.
Undocumented. Locally `main@{date}` reads your own reflog and fails on a fresh clone; on github.com the date is resolved server-side against the branch history, so it works on repositories you have never touched.

**Compare across forks.**
`https://github.com/OWNER/REPO/compare/main...OTHER_OWNER:main`. The three-part form `OTHER_OWNER:OTHER_REPO:REF` also resolves. `SHA~5...SHA` compares a commit with the one five before it.
If a branch and a tag share a name, the branch silently wins; write `tags/NAME` to force the tag.

**Three dots and two dots are different diffs.**
`A...B` diffs from the merge base of A and B to the tip of B, which is what pull requests show. `A..B` diffs the current tips, so it changes every time the base branch moves even when your branch has not.

**`?w=1` hides whitespace-only changes.**
Works on `/pull/N/files`, `/commit/SHA` and `/compare/` URLs. `?w=0` turns it back off. Undocumented as a URL parameter but working since 2011. The `.diff` and `.patch` endpoints ignore it.

**`?diff=split` and `?diff=unified` force the layout.**
Combines with the whitespace filter: `?diff=split&w=1`. Undocumented as a parameter; GitHub's docs only describe the gear menu.

<br>

### Files and lines

**`?plain=1` shows a rendered Markdown file as numbered source.**
Line anchors do nothing on the rendered view because it has no line numbers, which is why every Markdown permalink GitHub generates carries `?plain=1` before `#L14`. The parameter must come before the fragment: `README.md#L14?plain=1` is a fragment named `L14?plain=1` and does nothing.

**`#L10-L20` selects a range, and `y` pins it to a commit.**
Click a line number, shift-click another, then press `y` to rewrite the branch URL into a commit-SHA permalink. The page does not reload and nothing confirms it, so people press `y` and assume nothing happened.
A pasted permalink renders as a live code snippet only in the repository it came from. In another repository, or inside a committed Markdown file, it stays a bare URL.

**`/raw/` redirects to `raw.githubusercontent.com` and branch URLs are cached.**
`https://github.com/OWNER/REPO/raw/main/PATH` 302s to `https://raw.githubusercontent.com/OWNER/REPO/main/PATH`, which sits behind a cache a GitHub staff member put at five minutes. A fresh push appears to have no effect. Use a commit SHA in place of the branch name for an immutable, immediately correct URL.

**A gist raw URL is only permanent if it carries the revision SHA.**
`https://gist.githubusercontent.com/USER/ID/raw/FILENAME` is always-latest. `.../raw/REVISION_SHA/FILENAME` is frozen. Revisions are listed at `https://gist.github.com/USER/ID/revisions`. The dangerous one is the shorter one.

**Swap `/blob/` for `/blame/`.**
The `b` key does the same. A `.git-blame-ignore-revs` file in the repository root hides reformatting commits from web blame, so web blame and local blame can disagree. Append `~` to the SHA in the blame URL to see past the ignore file.

**`/find/BRANCH` is the file finder as a URL.**
The same thing `t` opens. It excludes `.git`, `.hg`, `.sass-cache`, `.svn`, `build`, `dot_git`, `log`, `tmp` and `vendor` by default, so a file under `vendor/` appears not to exist. Re-include with `linguist-generated=false` in `.gitattributes`.

<br>

### Downloads and feeds

**Archive URLs for any branch, tag or commit.**
`https://github.com/OWNER/REPO/archive/refs/heads/BRANCH.tar.gz`, `.../archive/refs/tags/TAG.zip` (handles slashes in tag names), `.../archive/FULL_SHA.zip`. These 302 to `codeload.github.com`, which locked-down networks often block. Wiki content is never included.

**`/releases/latest` and `/releases/latest/download/ASSET`.**
The first redirects to the current release page. The second resolves to that asset in whichever release is marked latest, so the filename must stay identical across releases. The download resolves in two hops, the second to a signed `release-assets.githubusercontent.com` URL with an expiry, so do not cache the final URL.

**Atom feeds for a repository, a branch, a single file, releases, tags, discussions, or a user.**
`/commits.atom`, `/commits/BRANCH.atom`, `/commits/BRANCH/PATH.atom` (for example `https://github.com/cli/cli/commits/trunk/README.md.atom`), `/releases.atom`, `/tags.atom`, `/discussions.atom`, `https://github.com/USERNAME.atom`.
Path-scoped feeds are titled identically to the repository feed, and every user feed is titled "GitHub Public Timeline Feed", so name them yourself in your reader.

**`/USERNAME.keys` and `/USERNAME.gpg`.**
One SSH public key per line, and an ASCII-armored PGP block. Useful for `authorized_keys` and for verifying signed tags. For an account with no GPG key, `.gpg` does not 404; it returns a well-formed empty block, so check the content and not the status code.

**`/USERNAME.png` is any user's avatar at any size.**
`https://github.com/USERNAME.png?size=100`. The github.com parameter is `size`; the redirect target uses `s`, and passing `s=` to github.com is ignored.

<br>

### Forms prefilled from the URL

**New issue.**
`https://github.com/OWNER/REPO/issues/new?title=New+bug&body=Describe+it&labels=bug,help+wanted&assignees=octocat&milestone=v1&projects=octo-org/1&template=bug.md`. `/issues/new/choose` opens the template chooser.
Permission is checked per parameter and failure is all-or-nothing: one parameter you lack permission for turns the whole URL into a 404.

**New pull request.**
`https://github.com/OWNER/REPO/compare/main...my-branch?quick_pull=1&title=Fix&labels=bug&template=release.md`. Same all-or-nothing rule; an oversized URL gives 414.

**New release.**
`https://github.com/OWNER/REPO/releases/new?tag=v1.0.1&target=main&title=v1.0.1&body=Notes&prerelease=1`. Almost nobody knows this one exists.

**Commits page filtered by author and date.**
`https://github.com/OWNER/REPO/commits/main?author=USERNAME&since=2025-01-01&until=2025-01-08`. A range that matches nothing renders normally with a "no commit history" message rather than an error, so a mistyped username looks like an empty week.

<br>

### Hidden repositories and graphs

**`OWNER/REPO.wiki.git` is a separate git repository.**
Clone it, edit locally, push. It never appears in a normal clone, never in any archive download, and search engines only index wikis on repositories with 500 or more stars that block public editing. Backing up the repository does not back up the wiki.

**The `.git` suffix exposes the smart-HTTP ref advertisement.**
`https://github.com/OWNER/REPO.git/info/refs?service=git-upload-pack` lists every ref including `refs/pull/*`, which a normal clone never fetches. Locally: `git fetch origin pull/ID/head:BRANCH`. The output is pkt-line binary, not JSON.

**The graph URLs and the caps they hit silently.**
`/network` shows only the 50 most recently pushed forks. `/graphs/contributors`, `/graphs/code-frequency` and `/graphs/traffic` exist, and since January 2024 the contributors graph drops addition and deletion counts on large repositories.

<br>

## 2. One-word swaps

Change one word in a github.com URL and the same repository becomes something else. Every one of these was loaded on 22 August 2026.

| Change | Result | What you should know |
|---|---|---|
| `github` → `github1s` | VS Code, read-only, in the browser | Private repos need a token pasted into the status bar. Same author runs `gitlab1s.com` and `npmjs1s.com`. |
| `github.com` → `github.dev`, or press `.` | GitHub's own editor with write access | No compute: no terminal, no builds. `>` opens it in a new tab. May not work on non-US keyboard layouts. |
| `github` → `gitingest` | The whole repository as one text file for a prompt | The web form skips files over 50 kB by default, so lockfiles and generated code drop out silently. `pip install gitingest` for the CLI. |
| `github.com` → `deepwiki.com` | A generated wiki with a table of contents, plus an MCP endpoint | Public repos only on the public endpoint. Private repos use `mcp.devin.ai` with a bearer token. |
| `github.com` → `gitmcp.io` | The repository as an MCP server, nothing to install | `gitmcp.io/OWNER/REPO` is scoped to one repository. `gitmcp.io/docs` is a floating endpoint the model can point anywhere; treat it as the riskier form. Pages sites: `OWNER.gitmcp.io/REPO`. |
| `github` → `gitdiagram` | An architecture diagram | It is an LLM reading the file tree and README, not static analysis of imports, so it can draw relationships that do not exist. |
| `github` → `gitreverse` | A synthetic "build this" prompt | Reads metadata, the root tree one level deep and the README. It is not a code ingest. |
| `github` → `gitpodcast` | A spoken walkthrough | The in-depth version requires sign-in. Runs on the maintainer's free-tier keys. |
| `github.com` → `github.gg` | Diagrams and a repository scorecard | Private repos need their GitHub App; three free reviews per installation. |
| prefix `pr.new/` | StackBlitz Codeflow | `https://pr.new/github.com/vuejs/docs`. Still labelled beta by StackBlitz. |
| `github` → `forgithub` | The index of every swap tool | Not checked for link rot; it still lists two dead services (see section 9). |
| `github` → `uithub` | Formerly a token-counted text view | **Now behind a GitHub sign-in wall, even for public repositories.** |

<br>

## 3. Keyboard and interface

**`?` lists the shortcuts for the page you are on.** The docs page says it lists only "some" shortcuts; the dialog is the authority. It does nothing while focus is in a text field.

**Turn off every single-letter shortcut at `/settings/accessibility`.** Deselect "Character keys". Modifier shortcuts such as `Ctrl+K` keep working, and so do the `Alt+Shift` sub-issue shortcuts, which become the only keyboard route for sub-issues.

**`t` is the fuzzy file finder.** Type `src/comp/butt` to reach `src/components/Button.tsx`. Nine directories are hidden by default (see `/find/` above).

**`y` rewrites the URL to a permalink.** See section 1. No visual confirmation.

**`.` opens github.dev, `>` opens it in a new tab.** You must be signed in.

**`Alt+Up` moves keyboard focus into a hovercard.** `Esc` returns focus. This is the only documented keyboard route to hovercard content. Hovercards can be turned off under Accessibility, in which case there is nothing to move into.

**The `g` pairs jump between tabs.** `g c` Code, `g i` Issues, `g p` Pull requests, `g a` Actions, `g w` Wiki, `g g` Discussions, `g s` Security (not Settings, which trips everyone), `g n` notifications, and inside a workflow run `g f` the workflow file. Press both keys quickly.

**`Alt`-click a label to exclude it.** On an issue list press `l`, then hold `Alt` and click. Plain clicking includes; nothing on screen hints that the modifier exists.

**`Alt`-click one file's caret to collapse every diff.** On Files changed, hold `Alt` and click the inverted caret in any file header. Announced in August 2018, never added to the shortcut docs; confirm it on the current page before relying on it.

**`Option`/`Alt`-click a collapsed thread to toggle every outdated or resolved thread.** Documented in the shortcut tables, not in the review docs, so almost nobody finds it. Open unresolved threads are unaffected.

**`Ctrl+.` then `Ctrl+NUMBER` fills a comment from a saved reply.** Create them at `/settings/replies`. Maximum 100, personal not shared, and the number is the position in your list, so reordering renumbers them.

**Select text and press `r` to quote it in a reply.** Quotes only the selection and appends to the reply box, so repeated presses stack.

**`Ctrl+G` inserts a suggestion block prefilled with the selected lines.** Click a line number, shift-click the last, then `Ctrl+G` (`Cmd+G` on Mac). The block is prefilled with exactly the lines you selected, so a suggestion spanning more lines deletes the extras when applied.

**The Viewed checkbox unticks itself when the file changes.** A rebase or force push can unmark many files at once and there is no bulk re-mark.

**`J` `K` `P` `N` `C` `T` drive the Files changed page.** `J`/`K` next and previous file in single-file mode, `P`/`N` between commits, `C` the commit filter, `T` the file filter. `Ctrl+Shift+Enter` submits a review. `J` and `K` do nothing in the all-files view.

**The review page now lives at `/pull/N/changes`.** `/files` redirects when the new experience is on, which became the default on 22 January 2026 with an opt-out. A colleague who opted out sees the old page.

**You can now comment on unchanged lines of a changed file.** Expand the diff, click `+` on the line. Still not possible on a file the branch did not touch.

**`Cmd+Enter` highlights a symbol and its occurrences in a code file.** `Shift+J` highlights the selected line. Depends on code navigation, which supports a fixed tree-sitter language list.

**The command palette is off by default.** Enable it in Feature preview, then `Ctrl+K` (`Ctrl+Alt+K` inside Markdown, where `Ctrl+K` means link). Prefixes: `>` commands, `#` issues and PRs, `@` users and repos, `/` files, `!` projects.

**`e`, `Shift+U`, `Shift+I`, `Shift+M` triage the notification inbox.** Done, unread, read, unsubscribe. Done is not storage: done notifications are kept for five months. The inbox search accepts `is:unread`, `reason:participating` and `repo:OWNER/REPO`.

**`Alt+Shift+C`, `A`, `P` manage sub-issues from the keyboard.** Create, add existing, edit parent. Issue pages only.

**Paste a URL over selected text and GitHub writes the Markdown link.** Ordinary paste fires it whether you wanted it or not. `Ctrl+Shift+V` pastes plain.

**`Shift+T` and `Shift+F` in Actions logs.** Timestamps and full-screen. Timestamps are off by default, so a run that looks instant may hide minutes of wait. Click a step's line number for a shareable log-line URL; the anchor format is undocumented, so copy it from the address bar.

**In Projects, `e` archives the selection immediately with no confirmation.** `Shift+Up` and `Shift+Down` extend the selection, so a stray keypress before `e` archives more than you meant. `Ctrl+Shift+\` opens the row menu.

**Drag the six-dot grip to reorder a task; click the issue icon to promote it.** Reordering works within one comment only. The tasklist block feature is retired; plain checklists remain.

**The Conversations menu on Files changed lists every unresolved thread.** Only the PR author or someone with write access can resolve a thread, so an outside contributor cannot tidy their own.

<br>

## 4. Search syntax

### Code search

**Regular expressions in slashes.** `/sparse.*index/`, and inside qualifiers: `path:/(^|\/)README\.md$/`, `language:rust symbol:/^String::to_.*/`. Escape every literal slash. `\n`, `\t` and `\x{hhhh}` work. Look-around assertions do not.

**Case-sensitive search with `(?-i)`.** Code search is case-insensitive by default and nothing warns you. `/(?-i)True/` forces it; the whole term becomes a regex, so escape metacharacters.

**`symbol:` searches definitions only.** `symbol:deleteRows` matches where it is defined, never where it is called. Some languages accept a class-name prefix; GitHub does not document which prefix belongs to which language. Supported languages: Bash, C, C#, C++, CodeQL, Elixir, Go, JSX, Java, JavaScript, Lua, PHP, Protocol Buffers, Python, R, Ruby, Rust, Scala, Starlark, Swift, TypeScript.

**`path:` globs, anchoring, and the quote trap.** `path:*.txt`, `path:src/*.js` anywhere, `path:/src/*.js` anchored to the root, `path:/src/**/*.js` to descend. Quoting disables globbing without warning: `path:"file?"` searches for the literal text `file?`.

**`content:` stops matching file paths.** `content:README.md` finds files whose text contains the string, excluding files merely named that. The fix for drowning in path hits when searching a filename-shaped term.

**`is:archived`, `is:fork`, `is:vendored`, `is:generated`.** Invert with `NOT`: `log4j NOT is:archived`. Forks are only indexed when they have more stars than the parent and at least one star themselves.

**`enterprise:` scopes across every organisation at once.** GitHub Enterprise Cloud only, generally available 5 November 2025.

**The index limits nobody tells you about.** Default branch only. Files over 350 KiB, empty files, files with any line over 4,096 bytes, binary and non-UTF-8 files are excluded. Lines over 1,024 characters are truncated. Exhaustive search is not supported, very large repositories may not be indexed, results cap at 100 across five pages, queries cap at 1,000 characters, there is no sorting, and you must be signed in. An absent result never proves the code does not exist.

**The REST code search API is a different, older engine.** It takes `in:file`, `in:path`, `filename:`, `extension:` and `size:`, which the website no longer documents, and cannot do regex or `symbol:`. `gh search code` wraps this legacy API, which is why its flags are `--filename`, `--extension` and `--match`.

<br>

### Issues and pull requests

**`reason:completed` and `reason:"not planned"`.** Since December 2024 there is a third close reason, duplicate, that matches neither, so a query built on two buckets silently loses those issues. Issues closed before close reasons existed, or via the API without a `state_reason`, have no reason at all.

**`-linked:pr` finds issues nobody is working on.** `repo:OWNER/REPO is:issue is:open -linked:pr`. Only a closing reference counts (`Closes #123` or a linked development item); a pull request that merely mentions the issue does not. `linked:issue` does the same on the pull request side.

**Four review qualifiers that mean four different things.** `review-requested:USER` is a live queue and stops matching the moment a review is submitted. `user-review-requested:@me` excludes requests that reached you through a team. `team-review-requested:ORG/TEAM`. `review-involves:USER` is the historical one.

**The `no:` family cannot be negated.** `no:label`, `no:assignee`, `no:milestone`, `no:project` work; `-no:label` does not, and GitHub says so. There is no `has:label` either; `has:` exists only for `has:funding-file` (repositories) and `has:sub-issue`.

**`interactions:`, `reactions:`, `comments:`.** `interactions:>2000` is reactions plus comments, so it conflates a popular request with a long argument. Pair with `sort:reactions-+1`.

**A comma inside `label:` means OR, a space means AND.** `label:bug,resolved` widens; `label:bug label:resolved` narrows. Nothing signals which you built.

**`assignee:*` matches any assignee, in one repository only.** Drop the `repo:` scope and it stops behaving.

**`draft:true`, `is:queued`, `archived:false`.** `archived:false` is what cleans up an organisation-wide dashboard after a round of archiving, and hardly anyone adds it.

**`type:` and `field.NAME:` filter by issue type and issue field.** `(type:"Bug" AND assignee:octocat) OR (type:"Feature" AND assignee:hubot)`. These are the largest additions to issue search since sub-issues and most lists omit them.

**`AND`, `OR` and parentheses, five levels deep.** That is a hard ceiling. Space between qualifiers still means AND. If you can see pull requests in more than 10,000 repositories, unscoped search must be limited with `org:`, `user:` or `repo:`.

**Sort by an individual reaction.** `sort:reactions-+1`, `sort:reactions--1` (two hyphens, because the reaction is named `-1`), `sort:reactions-heart`, `sort:reactions-tada`, `sort:reactions-thinking_face`. Code search has had no sorting since 10 April 2023.

**Saved searches live behind `saved:`.** Press `/` or `s`, type `saved:`, then click the plus icon in the "Saved queries" section. There is no route to this from Settings.

**Semantic and hybrid issue search.** REST `/search/issues` with `search_type=semantic` or `hybrid`; GraphQL `searchType: SEMANTIC`. Rate limited to 10 requests a minute, a third of the standard search limit.

<br>

### Repositories and commits

**The obscure half of repository search.** `mirror:true`, `topics:>3` (count, distinct from `topic:NAME`), `size:1000..2000` in kilobytes, `followers:>=10000`, `good-first-issues:>5`, `help-wanted-issues:>10`, `has:funding-file`.

**`props.PROPERTY:VALUE` searches organisation custom properties.** `org:NAME props.production:true`. Single organisation only. The repository dashboard at `github.com/repos` accepts the same syntax and saves views.

**Walk the commit graph.** `hash:SHA`, `parent:SHA` for its children, `tree:SHA` for commits referencing a tree, `merge:true`, `author-name:`, `committer-name:`, `author-date:>2024-01-01`. Default branch only, so a commit that lives only on a feature branch is unfindable.

**Every search API result set stops at 1,000.** 100 per page. When a query exceeds its time budget the API returns what it found with `incomplete_results: true` and a 200 status; code that ignores that flag reports a partial answer as complete.

<br>

## 5. Repository and platform

### CODEOWNERS

**`docs/*` is one level deep; `/docs/` is recursive.** GitHub's own example file says so. The file parses cleanly and the review request goes to the wrong owner for nested paths.

**Last match wins, not most specific.** A `* @org/everyone` catch-all at the bottom disables every specific rule above it. Put the catch-all first.

**Bad lines are skipped silently, and teams without write access are dropped.** No `\#` escaping, no `!` negation, no `[ ]` ranges. Over 3 MB and the file is not loaded at all. A team must have explicit write access to the repository to be a code owner.

**CODEOWNERS is read from the base branch.** A pull request that adds itself as an owner cannot self-approve into effect. Lookup order: `.github/`, root, `docs/`.

<br>

### Pull requests and merging

**Several pull request templates, chosen by URL.** Put them in `.github/PULL_REQUEST_TEMPLATE/` and link `...compare/main...branch?template=release.md`. Unlike issues, there is no chooser; without the parameter only the root-level template applies.

**Closing keywords work across repositories; the sidebar does not.** `Fixes octo-org/octo-repo#100`. Ignored entirely unless the pull request targets the default branch.

**Draft pull requests defer code-owner review requests until marked ready.** `gh pr ready` from the CLI. The polite way to push work in progress against a large CODEOWNERS.

**Auto-merge switches itself off when an outsider pushes.** The button only appears when the pull request cannot merge right now, so a repository with no required checks never shows it.

**Merge queue stalls silently unless workflows listen for `merge_group`.** Add `on: merge_group:` to every required workflow. Without it, "status checks will not be triggered" and the queue waits forever. The queue builds on temporary branches prefixed `gh-readonly-queue/`.

**A required check skipped by path filtering blocks the pull request forever.** Skipped is not passed and there is no timeout. Fix with a second workflow of the same job name that runs on the complementary paths and exits 0.

<br>

### Rules and releases

**Rulesets stack; they do not replace.** Several rulesets plus any legacy branch protection all apply, and "the most restrictive version of the rule applies". A half-finished migration leaves contributors blocked by a rule nobody remembers. Evaluate mode lets you watch a ruleset without enforcing it.

**Rulesets can require commit message and author email patterns.** Enterprise plans only for the metadata rules.

**Push rulesets apply across the whole fork network.** Restrict file paths, path length, extensions and size, with up to 200 entries each. This is the only rule family that reaches into repositories you do not own; a contributor can be blocked in their own fork.

**Require signed commits behaves differently in rulesets.** Rulesets check only commits not reachable from other branches; branch protection checks every commit on the branch.

**Prefill the release form.** See section 1.

**`.github/release.yml` shapes autogenerated release notes.** One `changelog:` key, `exclude.labels`, `exclude.authors`, and ordered `categories` with `title` and `labels`. The `*` label is a catch-all and categories are evaluated in order.

**Immutable releases lock the tag and burn the name.** Once published, the tag cannot be moved, assets cannot be added, and if you delete the release you can delete the tag but never reuse the name. Draft first, attach assets, then publish.

**A permanent URL for the newest asset.** `/releases/latest/download/NAME.zip`. The filename must be stable across releases.

<br>

### Pages, wiki, gists, discussions

**With Actions-based Pages, the `CNAME` file is ignored.** Only branch-based publishing writes or reads it. Set the custom domain in Settings; static site generators that emit `CNAME` into the build do nothing.

**`.nojekyll` bypasses the Jekyll build.** Without it, Jekyll silently drops every directory and file beginning with an underscore, which deletes `_next`, `_app` and `_assets` output. Sites may be no larger than 1 GB; Actions deploys escape the Jekyll build limit.

**Secret gists are unlisted, not private.** Anyone with the URL can read one, you can make a secret gist public but never the reverse, and a gist is a full git repository with history.

**Only Q&A categories accept answers, and triage role is enough to mark one.** 25 categories per repository. Switching a category's format is the usual fix.

**The Projects auto-add workflow ignores everything that already exists.** No backfill button. Add existing items by hand or through the API.

**Template repositories produce unrelated histories and reject LFS.** You cannot open a pull request between a template and a repository generated from it.

**Archiving freezes everything; deleting starts a 90-day clock and can take forks with it.** Forks of a public repository survive; deleting a private repository deletes its private forks.

<br>

## 6. Actions and automation

### Triggers and inputs

**Typed `workflow_dispatch` inputs give manual runs real form controls.** `type: choice` with `options`, `type: boolean`, `type: environment`. Up to 25 top-level inputs, 65,535 characters total.

**Use the `inputs` context, not `github.event.inputs`.** The latter turns booleans into strings, and `if: github.event.inputs.dry_run` is truthy for `"false"` because any non-empty string is truthy.

**Actions taken with `GITHUB_TOKEN` do not trigger further workflows.** A release workflow that pushes a tag with the default token will not fire the tag-triggered publish workflow, and nothing says why. Use a GitHub App token or a PAT for the push, or call the next workflow explicitly.

**`workflow_run` handlers run on the default branch.** A bare `actions/checkout` in one checks out the default branch, not the pull request's code.

**Six commit-message strings skip a run.** `[skip ci]`, `[ci skip]`, `[no ci]`, `[skip actions]`, `[actions skip]`, and the trailer `skip-checks: true`. They apply only to `push` and `pull_request`; `pull_request_target`, `workflow_dispatch` and `schedule` still fire.

**Scheduled workflows on a public repository switch themselves off after 60 days of inactivity.** The single biggest cause of abandoned-looking automation. The shortest schedule interval is five minutes, and runs drift during peak load.

**The dispatch API now returns the run id.** Since the 2026-03-10 API version, `POST .../dispatches` returns a body with the run id instead of `204 No Content`. Integrations that asserted on 204 broke.

<br>

### Concurrency and structure

**`concurrency.queue: max` queues up to 100 runs instead of cancelling pending ones.** Cannot be combined with `cancel-in-progress: true`.

**`matrix.include` merges into existing combinations unless it would overwrite a value.** `{color: green}` lands on every row; `{color: pink, animal: cat}` overrides it on the cat rows; a fully new combination is appended. Order matters.

**`background: true` runs a step asynchronously; `wait` and `wait-all` collect it.** Added 25 June 2026. Steps need an `id` to be waited on, and a background step's outputs are not available until the wait.

**Reusable workflows: `uses: OWNER/REPO/.github/workflows/X.yml@SHA` with `secrets: inherit`.** Workflow-level `env` does not cross the boundary; pass values through `with:`. Nesting is four levels deep.

**Composite actions need `shell:` on every `run` step.** Optional in a workflow, mandatory in `action.yml`, and omitting it is a hard failure.

**Naming one permission sets every other to `none`.** Adding `permissions: id-token: write` for OIDC removes `contents: read`, and `actions/checkout` then fails on a private repository. Always list `contents: read` alongside.

<br>

### Outputs, caches, artifacts

**Write outputs to files, with a heredoc for multiline values.** `echo "version=1.2.3" >> "$GITHUB_OUTPUT"`, `>> "$GITHUB_ENV"`, `>> "$GITHUB_PATH"`. For multiline, use a random delimiter; the docs warn that a delimiter appearing in the value breaks the file.

**`GITHUB_STEP_SUMMARY` renders Markdown on the run page.** 1 MiB per step, and exceeding it fails the step with an annotation.

**Job outputs drop anything that looks like a secret, and matrix jobs overwrite each other.** A version string that matches a masked value vanishes with only a warning in the log. Matrix jobs all write the same output key, so the last one wins; use artifacts for per-leg results.

**Caches are scoped by branch, and pull request caches are stranded on the merge ref.** A feature branch cannot restore a sibling's cache, and a cache created on `refs/pull/N/merge` is invisible to the branch after merge. Build the cache on the default branch so every branch can inherit it.

**Untrusted triggers get a read-only cache token.** Since the change, `pull_request_target` and similar workflows can restore but not save caches, with no opt-out and no error; the run succeeds and the cache never warms.

**`restore-keys` gives a warm cache on a lockfile change.** Test the hit with `!= 'true'`, never `!`, because `'false'` is a truthy string.

**Artifacts are immutable since v4.** Two jobs can no longer append to one artifact name. Give each matrix leg a unique name and merge on download with `pattern:` and `merge-multiple: true`.

**Download an artifact from a different run.** `actions/download-artifact` with `run-id:` and `github-token:`. The token is required even within the same repository.

**Environments gate a job behind human approval.** Free on public repositories; private repositories need Pro or Team. Only one of up to six required reviewers needs to approve.

<br>

### Security in workflows

**`pull_request_target` plus checking out the PR head is repository takeover.** The workflow runs with the base repository's secrets and a write token, and a `postinstall` script in the fork's `package.json` runs with them. Never check out `github.event.pull_request.head.sha` under `pull_request_target` unless the job does nothing privileged with the code.

**Never interpolate `${{ github.event.* }}` into a `run:` block.** The substitution happens before the shell sees the line, so quoting does not help. Pass the value through `env:` and reference the variable.

**Pin third-party actions to a full 40-character SHA.** `uses: actions/checkout@08c6903cd8c0fde910a37f88322edcfb5dd907a8 # v7.0.0`. Short SHAs are rejected. Dependabot updates the pin and rewrites the version comment; organisations can require SHA pins by policy.

**Debug logging without editing the workflow.** Set `ACTIONS_STEP_DEBUG` and `ACTIONS_RUNNER_DEBUG` to `true` as secrets or variables. Runner diagnostics go into the log archive, not the visible log.

**Tools.** `act` runs workflows locally and ignores `concurrency`, `run-name`, `environment` protections and `permissions`. `zizmor` audits workflows for injection and supply-chain bugs, and several of its audits need network access. `actionlint` type-checks expressions and the shell inside `run:`.

<br>

## 7. Security and supply chain

**OIDC subject claims changed format for repositories created after 15 July 2026.** A trust policy copied from an older repository silently fails to match. Check the `sub` claim in a test run before wiring a cloud role.

**Bypassing push protection on a public repository usually creates no alert.** User-level push protection is on by default and does not generate alerts when bypassed unless repository-level push protection is also on.

**CodeQL default setup stops scanning a dormant repository after six months.** No pushes and no pull requests for six months, and the weekly schedule is disabled. A finished project's last scan is its last scan.

**Dependabot pull requests read a different secret store.** Repository Settings, Secrets and variables, Dependabot. A workflow that passes on human pull requests fails on Dependabot's because the Actions secret is empty there.

**Dependabot for Actions needs `directory: "/"`, not `".github/workflows"`.** The intuitive path is valid config that finds nothing.

**Dependabot groups ignore security updates unless you say `applies-to: security-updates`.** And `cooldown` never delays a security fix, nor does `open-pull-requests-limit` cap one. Since 14 July 2026 version updates carry a default three-day cooldown.

**A preset auto-triage rule already dismisses some alerts on public repositories.** "Dismiss low impact issues for development-scoped dependencies" is on by default for public and off for private, the reverse of what people assume.

**Secrets found in public repositories go straight to the provider.** Partner-pattern matches are reported to the service, which typically revokes the key, and never appear in your Security tab. Free secret scanning covers history, all branches, issues, discussions, wikis and gists; validity checks need Team or Enterprise.

**`gh attestation verify` accepts almost anything unless you add policy flags.** `--deny-self-hosted-runners`, `--signer-repo`, `--signer-workflow`, `--source-ref`. The default predicate type is SLSA provenance, so an SBOM attestation needs `--predicate-type`. For air-gapped use: `gh attestation download`, `gh attestation trusted-root > trusted_root.jsonl`, then verify offline with `--custom-trusted-root`, remembering a saved root knows nothing about revocation.

**`actions/attest-build-provenance` is a wrapper; use `actions/attest`.** Permissions `id-token: write`, `contents: read`, `attestations: write`, plus `packages: write` for containers.

**Fine-grained tokens cannot do several things classic tokens can.** They cannot contribute to public repositories where you are not a member, cannot act as an outside collaborator, and max out at 366 days unless set to `none`. The creation page accepts query parameters to prefill a token.

**Immutable releases are one-way.** See section 5.

**Private vulnerability reporting is off by default and researchers cannot force it.** Enable at `Settings > Advanced Security`. Without it there is no private path and GitHub's own fallback advice is to open a public issue.

**GitHub is a CNA and issues CVEs in about 72 hours.** Draft a security advisory, click Request CVE. Rejected if another CNA already covers your project.

**One `SECURITY.md` in a public `.github` repository covers every repository you own.** The `.github` repository must be public; a private one is silently ignored.

**Vigilant mode turns silence into an explicit Unverified badge.** Without it, a commit forged with your name and email looks exactly like your normal unsigned commits.

**A deploy key is one repository, forever, with no expiry and no owner.** Usually not passphrase-protected, so a compromised server hands it over directly. Prefer a GitHub App installation token.

**Export an SPDX SBOM with one call.** `GET /repos/OWNER/REPO/dependency-graph/sbom`, or the Export SBOM button under Insights. Dependencies only, never dependents.

**`dependency-review-action` blocks a pull request that introduces a vulnerable or badly licensed package.** `allow-licenses` and `deny-licenses` are mutually exclusive.

<br>

## 8. Profile, README and badges

**`USERNAME/USERNAME` becomes your profile README.** Public, name still matching, README at the root, non-empty. Break any one and it silently vanishes.

**Organisation profile READMEs live at `.github/profile/README.md`.** A private `.github-private` repository with the same path gives a members-only twin. Under a personal account, `profile/README.md` does nothing.

**One public `.github` repository supplies default community files to every repository you own.** `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `FUNDING.yml`, `GOVERNANCE.md`, `SECURITY.md`, `SUPPORT.md`, issue and pull request templates. A private `.github` repository is ignored.

**`.github/README.md` quietly outranks the root README.** Search order is `.github/`, root, `docs/`. A short contributor note in `.github/README.md` replaces your real README on the repository page.

**Exactly nine HTML tags are filtered from GitHub Markdown.** `title`, `textarea`, `style`, `xmp`, `iframe`, `noembed`, `noframes`, `script`, `plaintext`. Everything animated in a profile README is therefore a static image URL.

**Every external README image is served through camo, and you can purge its cache.** Copy the `camo.githubusercontent.com/<hash>` URL and `curl -X PURGE` it. GitHub's docs say to use this sparingly; fix the origin's cache headers for a long-term solution.

**Dark and light images use `<picture>`.** `<source media="(prefers-color-scheme: dark)" srcset="dark.png">`. The `#gh-dark-mode-only` URL fragment is deprecated.

**The precise rule for a commit to appear on your contribution graph.** Author email linked to your account, standalone repository (forks never count), on the default branch or `gh-pages` (the one special case), and you are a collaborator or the repository is public or in an organisation you belong to. Private contributions, achievements, and the activity overview are three separate switches.

**The contribution grid is GraphQL only.** `contributionsCollection { contributionCalendar { weeks { contributionDays { date contributionCount } } } }`. There is no REST endpoint, which is why every card generator scrapes this.

**Six pins total, including repositories you do not own.** Any public repository you have contributed to in the last year; forks you do not own cannot be pinned.

**Achievement criteria are community-tracked, not documented.** Pull Shark at 2, 16, 128, 1024 merged pull requests; Pair Extraordinaire at 1, 10, 24, 48 co-authored commits; Starstruck for 16, 128, 512, 4096 stars on one repository; Galaxy Brain for accepted discussion answers; YOLO for merging without review; Quickdraw for closing within five minutes. GitHub still labels the feature a public preview.

**Build a badge from any JSON file.** `https://img.shields.io/badge/dynamic/json?url=<ENCODED URL>&query=$.version&label=version&color=blue`. JSONPath, URL-encoded; a wrong path yields a silently empty badge.

**Serve your own badge with four JSON keys.** `https://img.shields.io/endpoint?url=<ENCODED URL>` where the JSON has `schemaVersion: 1`, `label` (may be empty), `message` (may not be empty), and optional `color`. The JSON can be a static file in your repository.

**shields.io runs on donated GitHub rate limit.** Authorise read-only at `https://img.shields.io/github-auth` to lend yours; this is why GitHub badges sometimes go blank for everyone at once.

**Stat cards: generate them in your own Actions run.** `stats-organization/github-readme-stats-action@v2` writes the SVG to a branch. The shared `github-readme-stats.vercel.app` instance is unmaintained and was returning 503 when checked.

**The contribution snake is `Platane/snk/svg-only@v3` pushing to an `output` branch.** Keeping the generated SVG off `main` gives a stable raw URL without polluting history.

**Cron-updated READMEs stop updating after 60 days of repository inactivity.** The most common reason a snake, stat card or "now playing" widget freezes. Re-enable from the Actions tab or push any commit.

**Social preview: 1280 by 640 pixels, under 1 MB.** The upload refuses rather than resizes. Two hosts serve cards: `opengraph.githubassets.com` when you have no custom image, `repository-images.githubusercontent.com` when you do. User profiles get no card at all, only the avatar.

**Topics: 20 maximum, 50 characters each, lowercase with hyphens.** The API `PUT /repos/OWNER/REPO/topics` replaces the whole set; sending one topic deletes the other nineteen.

**`FUNDING.yml`: one organisation plus four developers under `github:`, four `custom:` URLs, default branch only.**

**Fix a wrong language bar with `.gitattributes`.** `*.rb linguist-language=Java`, `vendor/* linguist-vendored`, `dist/* linguist-generated`, `docs/* linguist-documentation`. Only `programming` and `markup` types count by default.

**Contributor avatar wall.** `https://contrib.rocks/image?repo=OWNER/REPO` with `max=`, `columns=`, `anon=1`. Avatars are base64-embedded, so the image is heavy.

**Social preview from a URL.** `https://socialify.git.ci/OWNER/REPO/image?theme=Light&pattern=Charlie%20Brown&description=1`. Its README warns the domain may change without notice, so do not hotlink it in production.

**GH Archive: every public event since 2011, hourly.** `https://data.gharchive.org/2015-01-01-15.json.gz`, and the `githubarchive` BigQuery dataset. The schema changed on 1 January 2015, and the Events API dropped payload fields on 7 October 2025.

**OSS Insight dashboards.** `https://ossinsight.io/analyze/OWNER/REPO`. Its star-based rankings are switched off because the public events firehose stopped emitting most star events.

<br>

## 9. Dead and changed

Listed so you stop recommending them.

| Tip as usually written | What is true on 22 August 2026 |
|---|---|
| Visit `/stargazers` or `/watchers` to see who starred a repo | Restricted to repository admins and collaborators since 30 June 2026. Logged out, a bare 404. The REST endpoints `/stargazers` and `/subscribers` are restricted the same way, and `/users/{user}/subscriptions` returns empty. |
| Embed a star-history chart with `api.star-history.com/svg?repos=...` | Returns an SVG saying GitHub restricted the data, for any repository you do not own. |
| Use `github-readme-stats.vercel.app` in your profile | Repository marked unmaintained; shared instance returning 503. Use the successor, or generate cards in your own Actions run. |
| `uithub.com` to get a repo as LLM text | Sign-in wall even for public repositories. Use `gitingest.com` or `gitmcp.io`. |
| `openrepowiki.xyz` | Redirects to a GoDaddy for-sale page. Still listed on forgithub.com. |
| `talktogithub.com` | Ad-parked. Still listed on forgithub.com. |
| Octotree is a free open-source extension | The shipping extension is proprietary with a Pro tier; the 23k-star repository does not build it. |
| Gitpod | Gitpod Classic was sunset on 15 October 2025; the company is now Ona and workspace content was not migrated. |
| Press `x` to select issues | Not a shortcut. `x` on an issue page means something else, and bulk selection is a checkbox. |
| `#gh-dark-mode-only` image URLs | Deprecated; use `<picture>`. |
| Cache on a pull request and reuse it after merge | Pull request caches live on the merge ref and are stranded. |
| `directory: ".github/workflows"` for Dependabot | Valid config that finds nothing; use `"/"`. |
| Tasklist blocks (`tasklist` fenced blocks) | Retired. Plain checklists remain. |
| `actions/attest-build-provenance` | Now a wrapper; call `actions/attest`. |
| The workflow dispatch API returns 204 | Returns a body with the run id since the 2026-03-10 API version. |

<br>

## How this was made

Eight research passes, one per area, each instructed to read the primary source for every claim and to record the exact form and the failure mode rather than the headline. One adversarial verification pass on the search-syntax area confirmed 17 of 27 entries verbatim and corrected 10 on secondary details; those corrections are applied above. The remaining seven areas carry the researchers' own source checks and are marked as such here: treat an undocumented trick as something to confirm on the day you need it.

Found an error or a trick that is missing? [Open an issue](https://github.com/Amey-Thakur/GITHUB-TRICKS/issues/new). A claim with a link to the source is the fastest way in.

<br>

## Related

**[GIT-GUIDE](https://github.com/Amey-Thakur/GIT-GUIDE)** covers Git itself: every command, with danger levels and undo. This repository covers the website, the platform and the tools around it.

<br>

## License

[MIT](LICENSE). Copyright (c) 2026 Amey Thakur.

<br>

<div align="center">

**[Amey Thakur](https://github.com/Amey-Thakur)** &nbsp;·&nbsp; [ORCID 0000-0001-5644-1575](https://orcid.org/0000-0001-5644-1575) &nbsp;·&nbsp; [LinkedIn](https://www.linkedin.com/in/amey-thakur/)

</div>

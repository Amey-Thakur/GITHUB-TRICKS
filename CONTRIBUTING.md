<div align="center">

# Contributing

**One rule: every claim arrives with the source that proves it.**

</div>

---

## What this repository is

A reference to GitHub that people can trust without checking. That only holds if every entry was read against a primary source, and if entries that stop being true get removed rather than repeated.

So the bar for a change is not "is this a good trick". It is **"can a stranger verify it in one click"**.

<br>

## Adding a trick

Open an [issue](https://github.com/Amey-Thakur/GITHUB-TRICKS/issues/new) or a pull request with four things:

| | Example |
|---|---|
| **The trick**, in one sentence | Append `.patch` to a pull request URL to get a `git am`-ready mbox |
| **The exact form**, copy-and-run | `https://github.com/OWNER/REPO/pull/123.patch` |
| **The catch**, which is the part that earns its place | It 302s to `patch-diff.githubusercontent.com`, so `curl` needs `-L` |
| **The source** | A link to docs.github.com, the GitHub changelog, the tool's own README, or a live request anyone can repeat |

A trick with no catch usually means the docs were skimmed rather than read. The failure mode, the limit, the silent behaviour: that is what makes an entry worth keeping.

<br>

## Sources that count

> [!TIP]
> In rough order of strength: **GitHub's documentation** and **changelog**, the **tool's own README** for third-party tools, then a **reproducible live request** with the command and its output. A blog post is a lead, not a source. Another tips list is not a source at all.

If a behaviour is real but undocumented, say so in the entry. Several already do, because undocumented tricks can disappear without notice and a reader deserves to know which ones those are.

<br>

## Reporting something that broke

This is the most valuable contribution here, and the least common.

GitHub retires things quietly. `/stargazers` started returning 404 to the public, `github-readme-stats` stopped being maintained, `uithub.com` grew a login wall, and every tips list on the internet still recommends all three.

If an entry no longer works, open an issue with what you ran and what came back. Confirmed cases move to [Dead tricks](README.md#dead-tricks) rather than being deleted, so nobody rediscovers them the hard way.

<br>

## House style

- **Every word earns its place.** No filler, no throat-clearing, no "in today's fast-paced world".
- **Show the exact thing to type**, in a fenced block so it gets a copy button.
- **Live links** point at a real repository. Use `Amey-Thakur/GITHUB-TRICKS` where the trick demos on it, `cli/cli` where the trick needs a repository with real history.
- **CAPITALS** mark placeholders. `{owner}` and `{repo}` in a `gh` command are literal and must stay that way.
- **No em dashes.** Commas, full stops, parentheses or a middot instead.
- **Dates on anything volatile.** Copilot and Codespaces move monthly; an entry without a date is a trap for a future reader.

<br>

## Not accepted

- Tricks copied from another list without independently checking them.
- Anything requiring a paid third-party service to be useful.
- Tools that are dead, parked, or now behind a login wall, unless they are being added to [Dead tricks](README.md#dead-tricks).
- Self-promotion for an unrelated project.

<br>

<div align="center">

By contributing you agree your work is licensed under the [MIT License](LICENSE),
and that you will follow the [Code of Conduct](CODE_OF_CONDUCT.md).

</div>

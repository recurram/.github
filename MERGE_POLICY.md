# Merge policy

This document describes how pull requests are merged across repositories in the [Twilic organization](https://github.com/twilic). It applies to every repository unless a repository explicitly documents a narrower exception.

## Merge method

**Squash merge** is the default for every pull request on the default branch.

- Prefer squash unless there is a clear reason to do otherwise (for example, preserving a carefully ordered commit series that must land verbatim).
- **Rebase merge** may be enabled in repository settings but is not part of day-to-day workflow. Use it only when squash would lose necessary history and a linear branch is still required.
- Do not use **merge commits** on the default branch.
- Organization [rulesets](https://docs.github.com/en/organizations/managing-organization-settings/managing-rulesets-for-repositories-in-your-organization) may allow rebase merge; they should not encourage merge commits.

## Squash commit message

The squash commit message becomes the single commit on the default branch. Keep it accurate, concise, and suitable for `git log`.

### Human-authored pull requests

Repositories use **Pull request title and description** as the default squash commit message. GitHub pre-fills the squash dialog from the PR title (subject) and description (body).

- Keep the PR title in [Conventional Commits](https://www.conventionalcommits.org/) form when possible (see [CONTRIBUTING.md](./CONTRIBUTING.md)).
- The PR description may flow into the squash body when it adds durable context (for example, a breaking-change note). Trim anything that does not belong in `git log`: review threads, checklists, issue templates, or WIP notes.
- Edit the pre-filled squash message before merging whenever the default text is too long or noisy.

### Bot-authored pull requests

Pull requests opened by automation (for example **Dependabot**, **Renovate**, or GitHub Actions bots) often include long Markdown in the PR description. With **Pull request title and description** as the squash default, GitHub will copy that Markdown into the squash dialog. That content must **not** land on the default branch.

When squashing a bot pull request, always edit the pre-filled message:

1. Set the squash commit **subject** to a short, Conventional-Commits-style line that describes the change (often the PR title after light editing).
2. In the squash commit **body**, include only attribution footers that use these forms:
   - `Signed-off-by: Name <email@example.com>`
   - `Co-authored-by: Name <email@example.com>`
3. Do **not** include any other Markdown in the squash message: no release notes, dependency tables, compare links, issue/PR lists, HTML comments, or boilerplate from the bot template.

Example squash message for a bot PR:

```text
chore(deps): bump example from 1.2.3 to 1.2.4

Signed-off-by: Dependabot <49699333+dependabot[bot]@users.noreply.github.com>
```

If a bot did not add a `Signed-off-by` or `Co-authored-by` line and none is required, the body may be omitted entirely.

## Repository settings

Each repository should align GitHub settings with this policy:

| Setting                         | Recommendation                                     |
| ------------------------------- | -------------------------------------------------- |
| Allow merge commits             | Off                                                |
| Allow squash merging            | On                                                 |
| Allow rebase merging            | On (available; use only when squash is unsuitable) |
| Default commit message (squash) | **Pull request title and description**             |

Squash is the everyday path; rebase remains enabled for exceptional cases. Do not rely on merge commits for routine integration.

## Dependabot and org defaults

Shared Dependabot defaults may live in this repository’s [`dependabot.yml`](./dependabot.yml) when present. That file controls Dependabot branch commits and PR metadata; it does not replace the squash-message rules above. Maintainers are still responsible for editing the squash message at merge time (or via automation) so only allowed footers appear in the final commit.

## Automation (optional)

If a repository adds a workflow to normalize squash messages for bot pull requests, define the reusable workflow here under `.github/workflows/` and call it from individual repositories. Automation should implement the same rules as this document: Conventional-Commits-style subject, and body limited to `Signed-off-by` / `Co-authored-by` lines.

## Related documents

- [CONTRIBUTING.md](./CONTRIBUTING.md) — commit message conventions for this repository
- [GitHub: Configuring commit squashing](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/configuring-commit-squashing-for-pull-requests)

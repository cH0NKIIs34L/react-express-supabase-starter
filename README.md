# dev-prompts

A centralized collection of AI prompt templates for developer workflows — branch naming, commit messages, pull requests, GitHub issues, and feature implementation. Designed to be pulled into any project repo as a git subtree so updates propagate everywhere with a single command.

---

## Folder Structure

```
dev-prompts/
├── github/
│   ├── 1-new-branch.md       ← generate a branch name + git commands
│   ├── 2-commit-message.md   ← write a conventional commit message
│   ├── 3-pull-request.md     ← generate full PR fields and body
│   └── fill-issue.md         ← fill a GitHub issue template
└── workflow/
    ├── 1-implement.md        ← implement an issue + verification steps
    └── 2-commit-pr.md        ← commit staged files and open a PR
```

---

## Prompts

### `github/1-new-branch.md`
Paste a plain-text description of your change. The prompt determines the branch type, generates a name following `<type>/<short-description>` conventions, and outputs the exact git commands to create and publish it.

### `github/2-commit-message.md`
Describe what you changed and why. The prompt writes a Conventional Commits subject line, decides whether a body is needed, and outputs the full commit message ready to paste — plus an ASCII directory tree of affected files and file artifacts to download.

### `github/3-pull-request.md`
Provide the branch name, target branch, summary, implementation details, testing done, and linked issue. The prompt generates the PR title, body (Summary / What / Why / How / Testing / Checklist), labels, milestone, and squash-merge commit message.

### `github/fill-issue.md`
Paste a GitHub issue template and a plain-text description of the issue. The prompt fills every field, infers the title, scope, and type, and outputs a complete issue ready to paste into GitHub.

### `workflow/1-implement.md`
Paste an issue with an acceptance criteria checklist. The prompt implements the required changes and gives you a numbered list of terminal commands and outputs to verify each criterion passes.

### `workflow/2-commit-pr.md`
Run after staging your files. The prompt reads your staged diff alongside the rules in `prompts/` and `.github/` to generate multiple atomic commit messages (each with its own directory structure) and the full PR fields.

---

## Using These Prompts

Open the relevant `.md` file, paste its contents into your AI chat (Claude, Copilot, etc.), then fill in the `# input:` section at the top with your context before sending.

The numbered prefixes (`1-`, `2-`, `3-`) reflect the order you'd use them in a typical feature workflow:

```
1-new-branch       → create your branch
1-implement        → build the feature
2-commit-message   → write your commit
2-commit-pr        → commit staged files + open PR
3-pull-request     → fill out the PR
fill-issue         → file a GitHub issue at any point
```

---

## Adding to a New Repo (git subtree)

This is the recommended way to consume these prompts in a project repo. The files live inside your repo (no extra tooling needed for collaborators), but updates are pulled from one source.

**One-time setup:**

```bash
# 1. Add dev-prompts as a named remote
git remote add dev-prompts https://github.com/cH0NKIIs34L/dev-prompts.git

# 2. Pull the contents into a prompts/ folder
git subtree add --prefix=prompts dev-prompts main --squash
```

This creates two commits in your repo — a squash commit and a merge commit. That's expected behavior for `git subtree` and should not be rebased away, as the metadata is used for future pulls.

**Verify the remote was added:**

```bash
git remote -v
```

You should see `dev-prompts` listed alongside `origin`.

---

## Pulling Updates

Whenever this repo is updated, run this in any consuming repo to sync:

```bash
git subtree pull --prefix=prompts dev-prompts main --squash
```

This merges only the changes from `dev-prompts` into your `prompts/` folder without touching anything else in your repo.

---

## Updating a Prompt

1. Edit the file directly in this (`dev-prompts`) repo
2. Commit and push to `main`
3. In each consuming repo, run the pull command above

If you edit a prompt file directly inside a consuming repo (e.g. `fullstackjs-shopping-cart/prompts/`), those changes will be overwritten the next time you pull. Always make prompt edits here.

---

## Removing from a Repo

If you ever want to detach a repo from this subtree:

```bash
# Remove the prompts folder
git rm -r prompts/
git commit -m "chore(prompts): remove dev-prompts subtree"

# Optionally remove the remote
git remote remove dev-prompts
```

---

## Conventional Commits Reference

These prompts follow the [Conventional Commits](https://www.conventionalcommits.org) spec. Quick reference:

| Type       | When to use                                |
| ---------- | ------------------------------------------ |
| `feat`     | New feature or functionality               |
| `fix`      | Bug fix                                    |
| `chore`    | Maintenance, dependencies, config, tooling |
| `refactor` | Code restructuring without behavior change |
| `docs`     | Documentation only                         |
| `test`     | Adding or updating tests                   |
| `perf`     | Performance improvements                   |
| `ci`       | CI/CD pipeline or workflow changes         |
| `style`    | Formatting, no logic change                |
| `revert`   | Reverting a previous commit                |

Format: `<type>(<scope>): <subject>`
- Imperative mood — "add" not "added"
- Max 50 characters for subject line
- No period at the end
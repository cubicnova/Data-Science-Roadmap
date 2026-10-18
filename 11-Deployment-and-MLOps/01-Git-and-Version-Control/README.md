# 01 — Git and Version Control

Learn to track changes, collaborate safely, and maintain a clear history for data science and machine-learning projects.

## Learning objectives

By completing this module, you will be able to:

- Explain version control, Git, repositories, commits, branches, and remotes.
- Configure Git and create a repository from the command line.
- Stage selectively, commit meaningful snapshots, inspect history, and compare changes.
- Use `.gitignore` to keep generated files, secrets, and large datasets out of version control.
- Create branches, merge changes, and resolve simple merge conflicts.
- Work with GitHub remotes using clone, fetch, pull, and push.
- Recover from common mistakes using restore, revert, reflog, and carefully chosen reset operations.
- Apply practical Git workflows to notebooks, datasets, model artifacts, and collaborative ML projects.

## Why version control matters in data science

A data project includes more than source code: notebooks, configuration, documentation, tests, data schemas, and pipeline definitions all change over time. Git helps you answer: *what changed, why did it change, who changed it, and how can we return to a known-good state?*

Git stores snapshots and history locally. A remote such as GitHub enables collaboration and off-machine sharing, but Git and GitHub are not the same thing.

## The three areas to understand

- **Working tree:** the files you are editing.
- **Staging area (index):** the exact changes selected for the next commit.
- **Repository history:** committed snapshots and their parent relationships.

Typical loop:

```text
Edit files → git status → git diff → git add → git diff --staged
→ git commit → git push
```

## Suggested learning path

1. Read the concepts and commands.
2. Run `examples.ipynb` from top to bottom. It uses a temporary local Git repository and does not push anything online.
3. Complete `practice.md` before opening `solution.md`.
4. Finish the mini-project and apply the checklist to your own roadmap folder.

## Essential commands

| Goal | Command |
|---|---|
| Check installation | `git --version` |
| Set identity | `git config --global user.name "Your Name"` |
| Set email | `git config --global user.email "you@example.com"` |
| Create repository | `git init` |
| Copy a remote repository | `git clone URL` |
| Inspect state | `git status` |
| Inspect unstaged changes | `git diff` |
| Stage a file | `git add path/to/file` |
| Inspect staged changes | `git diff --staged` |
| Commit | `git commit -m "Describe the change"` |
| Review history | `git log --oneline --graph --decorate --all` |
| List branches | `git branch` |
| Create and switch branch | `git switch -c feature/name` |
| Merge branch | `git merge feature/name` |
| List remotes | `git remote -v` |
| Download remote updates only | `git fetch origin` |
| Integrate remote updates | `git pull` |
| Upload commits | `git push` |

## Installation and setup

Install Git from the official [Git website](https://git-scm.com/downloads). On Windows, Git for Windows includes Git Bash; PowerShell and Windows Terminal also work.

Verify installation:

```powershell
git --version
```

Set your commit identity (use the name and email you want recorded in commit metadata):

```powershell
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list --show-origin
```

Your commit email may become visible in public repository history. Use an appropriate GitHub-provided noreply address if desired.

## `.gitignore` for data science

Example:

```gitignore
# Python
__pycache__/
*.py[cod]
.venv/
venv/
.ipynb_checkpoints/

# Secrets and local configuration
.env
.env.*
!.env.example

# Generated outputs and large artifacts
outputs/
models/
*.joblib
*.pkl
*.h5
*.onnx

# Local data: keep schema/sample data only when approved
data/raw/
data/processed/
```

Only ignore model artifacts and data when that matches your project policy. If an ignored file is already tracked, adding it to `.gitignore` does not untrack it automatically. Secrets should be revoked/rotated if committed; deleting them in a later commit does not erase history.

## Branching workflow

A simple feature workflow:

```bash
git switch main
git pull --ff-only
git switch -c docs/update-git-module
# edit files and test
git status
git diff
git add README.md examples.ipynb practice.md solution.md
git diff --staged
git commit -m "Add Git learning module"
git push -u origin docs/update-git-module
```

Open a pull request on GitHub and merge after review. The exact branch and review rules depend on the repository.

## Undo carefully

- `git restore file.txt`: discard unstaged edits to a tracked file. This is destructive for those edits.
- `git restore --staged file.txt`: unstage a file but keep its working-tree edits.
- `git revert COMMIT`: create a new commit that reverses an earlier commit; often appropriate for shared history.
- `git reset --soft HEAD~1`: move the current branch back one commit but keep changes staged.
- `git reset --hard`: discard tracked working-tree/index changes and move the branch; potentially destructive.
- `git reflog`: inspect recent local reference movements and help recover lost commit references.

Before undoing, run `git status` and inspect the exact target. Avoid rewriting published/shared history unless the team explicitly agrees.

## Common issues

### `git push` is rejected (non-fast-forward)
First preserve local work, then inspect remote changes:

```bash
git status
git fetch origin
git log --oneline --graph --decorate --all
git pull --rebase origin main
# resolve conflicts if needed, test, then:
git push origin main
```

Use the correct branch name. Do not force-push as a first fix.

### Merge conflict
Open each conflicted file, choose the intended combined content, remove conflict markers, test, then:

```bash
git add path/to/resolved-file
git commit
```

To abandon an in-progress merge, `git merge --abort` may restore the pre-merge state.

### A secret or large dataset was committed
Stop further exposure, rotate/revoke secrets immediately, notify the project owner, and follow the organization's history-cleanup process. `.gitignore` prevents future accidental adds but does not remove past history or revoke a credential.

## Mini-project: version a small ML project

Create a repository containing:
- `README.md`, `src/`, `tests/`, `notebooks/`, and a `.gitignore`.
- A small synthetic dataset generator or schema, not private/raw production data.
- At least five clear commits.
- A feature branch and a merged change.
- A documented recovery exercise using a disposable repository.
- A remote workflow plan (clone/fetch/pull/push) without publishing secrets or large artifacts.

## Definition of done

- [ ] Git identity and default branch are configured.
- [ ] Commits describe logical changes.
- [ ] Staged changes are inspected before committing.
- [ ] `.gitignore` excludes local environments, secrets, and unapproved data/artifacts.
- [ ] A branch was created and merged.
- [ ] History and diffs can be read.
- [ ] Recovery commands are understood before use.
- [ ] No credentials, private data, or accidental large files are committed.

## Further reading

- [Pro Git — About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)
- [Pro Git — Git Basics](https://git-scm.com/book/en/v2/Git-Basics)
- [GitHub Docs — Getting changes from a remote repository](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository)

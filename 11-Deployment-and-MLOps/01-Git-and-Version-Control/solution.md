# Solutions — Git and Version Control

Reference answers and command examples. Use a disposable repository for destructive exercises. Commands that contact a remote require a configured URL and suitable permissions.

## 01. Version control fundamentals
Git records snapshots and their history, supports comparison and recovery, and lets people work on separate branches. Manual folder copies are hard to compare, easy to overwrite, and do not reliably explain who changed what or why.

## 02. Git vs GitHub
Git is the distributed version-control tool. GitHub is a hosted collaboration platform for Git repositories, pull requests, issues, and automation. Git commits and branches can be created offline; pushing requires network access and a remote.

## 03. Repository anatomy
The working tree contains current files; the index/staging area selects the next snapshot; the local repository stores commits and history; a remote is another repository, often hosted online.

## 04. Install verification
```bash
git --version
```
Record the output; exact versions differ by installation.

## 05. Identity configuration
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list --show-origin
```
The identity is commit metadata, not necessarily the same as an authenticated GitHub login.

## 06. Initialize a repository
```bash
mkdir git-practice
cd git-practice
git init -b main
git status
```
On older Git versions that do not support `git init -b`, use `git init` then `git branch -M main`.

## 07. First commit
```bash
echo "# Git Practice" > README.md
git add README.md
git diff --staged
git commit -m "Add project README"
```

## 08. Status interpretation
A modified tracked file appears as a change not staged or staged, depending on whether `git add` has been run. A new untracked file is not yet part of the next commit.

## 09. Diffs
```bash
git diff             # unstaged changes
git diff --staged    # staged changes
```
The staged diff is the proposed next commit; inspect it before committing.

## 10. Selective staging
```bash
git add README.md
git status
git diff --staged
```
Selective staging keeps unrelated changes out of a commit and improves reviewability.

## 11. Commit messages
Examples: `Add customer churn data dictionary`; `Fix missing-value handling in training pipeline`; `Document model evaluation workflow`.

## 12. History
```bash
git log --oneline --graph --decorate --all
```
The short hash identifies a commit, the message summarizes it, branch/tag labels show references, and the graph depicts ancestry.

## 13. Show a commit
```bash
git show HEAD
```
It displays commit metadata and the patch for the latest commit.

## 14. Ignore patterns
```gitignore
__pycache__/
*.py[cod]
.venv/
venv/
.ipynb_checkpoints/
.env
.env.*
!.env.example
outputs/
models/
*.joblib
*.pkl
```
Adapt rules to the project's data/artifact policy.

## 15. Already-tracked file
`.gitignore` affects untracked files. To remove a file from the index but keep it on disk:
```bash
git rm --cached path/to/file
git commit -m "Stop tracking local generated file"
```
If the file contains a secret, rotate/revoke it and follow history cleanup; untracking alone is not enough.

## 16. Branch creation
```bash
git switch -c feature/add-data-dictionary
git branch --show-current
```

## 17. Parallel work
```bash
git switch -c feature/new-metric
# edit, stage, commit
git add .
git commit -m "Add model evaluation metric"
git switch main
# make a different commit on main
```
Each branch points to its own line of development; commits remain in history and can later be merged.

## 18. Fast-forward merge
```bash
git switch main
git merge feature/add-data-dictionary
git log --oneline --graph --decorate --all
```
A fast-forward is possible when `main` has not diverged from the feature branch.

## 19. Merge conflict
Git inserts markers such as `<<<<<<<`, `=======`, and `>>>>>>>`. Edit the file into the intended final form, remove markers, test, then:
```bash
git add path/to/file
git commit
```
To abandon an in-progress merge, use `git merge --abort` when appropriate.

## 20. Delete branch
```bash
git branch -d feature/add-data-dictionary
```
Git protects against deleting an unmerged branch with `-d`. `-D` forces deletion and can discard a useful branch reference; use it only when certain.

## 21. Clone
```bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
git remote -v
git branch --show-current
```
Replace the URL with the real repository URL.

## 22. Remote inspection
```bash
git remote -v
```
Fetch URLs are used to retrieve data; push URLs are used to send commits. They may be the same or different.

## 23. Fetch vs pull
`git fetch origin` downloads remote refs and objects without integrating them into the current branch. `git pull` fetches and then integrates (usually by merge or rebase, depending on configuration). Inspect status and history before integration.

## 24. Push with upstream
```bash
git push -u origin feature/name
```
This publishes the branch and configures upstream tracking so later `git push`/`git pull` can infer the remote branch.

## 25. Non-fast-forward rejection
A cautious flow:
```bash
git status
git fetch origin
git log --oneline --graph --decorate --all
git pull --rebase origin main
# resolve conflicts, run tests
git push origin main
```
Use the actual branch name. Commit or safely stash local changes before rebasing. Do not use force-push as the default fix.

## 26. Unstage a file
```bash
git restore --staged path/to/file
```
The edits remain in the working tree.

## 27. Discard an edit
First inspect:
```bash
git diff
```
Then, only if the changes are definitely disposable:
```bash
git restore path/to/file
```
This discards unstaged edits to that file.

## 28. Revert vs reset
`git revert COMMIT` adds a new commit that reverses the target change and is usually safer for shared history. `git reset` moves the current branch reference and may rewrite local history; `--hard` also discards tracked working-tree/index changes. Prefer revert for commits already shared.

## 29. Reflog recovery
```bash
git reflog
git switch -c recovery/old-work <commit-hash>
```
Create a new branch at the desired reflog commit before doing further cleanup. Reflog is local and entries eventually expire.

## 30. Tags
```bash
git tag -a v0.1.0 -m "First practice release"
git show v0.1.0
```
A branch normally advances as new commits are added; a tag names a specific point, often a release.

## 31. Notebook hygiene
Clear noisy cell outputs where useful, keep notebooks focused, move reusable logic into `.py` modules, avoid committing huge embedded outputs, review diffs, and ensure notebooks run from a clean kernel. Do not blindly clear outputs when they are meaningful evidence or teaching material.

## 32. Data and artifacts
Commit schemas, synthetic examples, small approved fixtures, code, and documentation. Usually exclude raw private data, large generated datasets, credentials, and reproducible model binaries; use approved object storage, dataset versioning, or Git LFS when suitable.

## 33. Secret exposure
Immediately revoke/rotate the token, notify the owner/security team, inspect access logs, remove it from current files, and follow approved history-removal procedures. Coordinate with collaborators because history rewriting affects clones. Never assume deleting the latest line removes exposure.

## 34. Large file handling
Git stores repository history, so repeatedly changing large binaries can inflate clones and history. Consider object storage or a data/versioning system; Git LFS can track large files via pointers but requires suitable hosting and quotas.

## 35. Pull request workflow
Review scope and intent, inspect diffs, run tests/linting, check secrets and data, verify docs, assess backward compatibility, confirm CI passes, request changes if needed, and merge according to team policy.

## 36. Commit granularity
Mixed commits are difficult to review, revert, cherry-pick, and bisect. Split work into logical commits (for example, formatting only, model behavior change, documentation) and verify each staged diff.

## 37. Conflict prevention
Use short-lived branches, coordinate ownership of shared notebooks/config files, extract reusable code from notebooks, communicate interface changes, and pull/fetch regularly when the working tree is safe.

## 38. Git hooks and CI
A pre-commit hook runs locally before a commit and can be bypassed or differ between developers unless standardized. CI runs server-side or in a shared automation environment on pushed changes/pull requests and provides repeatable checks.

## 39. Mini-project
One possible sequence:
```bash
git init -b main
# create README.md, .gitignore, src/, tests/
git add README.md .gitignore src tests
git commit -m "Create project scaffold"
# add data schema
git add data/schema.json
git commit -m "Add input data schema"
git switch -c feature/validate-inputs
# implement and test validation
git add src tests
git commit -m "Validate input records"
git switch main
git merge feature/validate-inputs
git log --oneline --graph --decorate --all
```
Add at least two further logical commits and a safe recovery exercise in a disposable repository.

## 40. Troubleshooting drill
Start with read-only diagnostics:
```bash
git status
git branch --show-current
git remote -v
git diff
git diff --staged
git log --oneline --graph --decorate --all
git fetch origin
```
Then decide whether to commit, stash, resolve conflicts, or rebase/merge. Avoid reset, clean, and force-push until the state and consequences are understood.

# Practice — Git and Version Control

Work in a disposable practice directory or repository. Do not experiment with destructive commands in a valuable working tree. Complete the exercises before consulting `solution.md`.

## 01. Version control fundamentals

Explain three problems Git solves compared with keeping copies such as `project-final-final-v3/`.

## 02. Git vs GitHub

Explain the difference between Git and GitHub and give one example of using Git offline.

## 03. Repository anatomy

Describe the working tree, staging area, local repository, and remote.

## 04. Install verification

Run `git --version`. Record the version and operating system.

## 05. Identity configuration

Set your Git name/email and verify them with `git config --list --show-origin`.

## 06. Initialize a repository

Create a folder called `git-practice`, initialize Git, and inspect the generated status.

## 07. First commit

Create `README.md`, stage it, and make a commit with a descriptive message.

## 08. Status interpretation

Change a tracked file and create an untracked file. Explain how `git status` distinguishes them.

## 09. Diffs

Make both staged and unstaged changes. Use `git diff` and `git diff --staged` to compare them.

## 10. Selective staging

Stage only one of two modified files. Explain why staging is useful.

## 11. Commit messages

Rewrite `update`, `fix`, and `changes` as meaningful commit messages for realistic project changes.

## 12. History

Use `git log --oneline --graph --decorate --all` and explain the fields in the output.

## 13. Show a commit

Use `git show HEAD` and identify the files and changes included in the latest commit.

## 14. Ignore patterns

Write a `.gitignore` for Python, Jupyter checkpoints, virtual environments, `.env`, and generated model artifacts.

## 15. Already-tracked file

Explain why adding a tracked file to `.gitignore` does not stop tracking it. Show how to untrack it while retaining the local copy.

## 16. Branch creation

Create `feature/add-data-dictionary`, switch to it, and confirm the current branch.

## 17. Parallel work

Make a change on a feature branch and a different change on `main`. Explain how branches isolate work.

## 18. Fast-forward merge

Merge a branch whose commits are directly ahead of `main`; inspect the history.

## 19. Merge conflict

Create a conflict by editing the same line on two branches. Resolve it and document the final content.

## 20. Delete branch

Delete a merged local feature branch and explain why an unmerged branch may be protected from deletion.

## 21. Clone

Clone a repository from a URL and identify the default remote and checked-out branch.

## 22. Remote inspection

Use `git remote -v` and explain fetch and push URLs.

## 23. Fetch vs pull

Explain the difference between `git fetch` and `git pull`; demonstrate with a local or practice remote.

## 24. Push with upstream

Explain `git push -u origin feature/name` and what tracking relationship it establishes.

## 25. Non-fast-forward rejection

Write a safe sequence to inspect remote changes and integrate them before retrying a push.

## 26. Unstage a file

Stage a file, then unstage it without losing its edits.

## 27. Discard an edit

In a disposable repository, discard an unstaged edit. Explain why you should inspect `git diff` first.

## 28. Revert vs reset

Compare `git revert` with `git reset` and recommend one for a commit already shared with teammates.

## 29. Reflog recovery

Describe how `git reflog` can help find a commit after a branch pointer moved.

## 30. Tags

Create an annotated tag `v0.1.0` and explain how tags differ from branches.

## 31. Notebook hygiene

List good practices for reviewing notebook diffs and avoiding noisy output changes.

## 32. Data and artifacts

Decide which of raw data, synthetic samples, trained model files, and data schemas belong in Git. Justify each decision.

## 33. Secret exposure

A cloud token was committed and pushed. List the immediate response steps.

## 34. Large file handling

Explain why Git is not always suitable for multi-gigabyte datasets and name an alternative strategy.

## 35. Pull request workflow

Write a checklist for reviewing a teammate's feature branch before merging.

## 36. Commit granularity

A commit mixes formatting, model changes, and unrelated docs. Explain why this is hard to review and how to split it.

## 37. Conflict prevention

Propose three team practices that reduce merge conflicts in notebooks and shared configuration.

## 38. Git hooks and CI

Explain the difference between a local pre-commit hook and a CI check.

## 39. Mini-project

Create a toy ML project with `.gitignore`, at least five commits, a feature branch, a merge, and a documented history.

## 40. Troubleshooting drill

Given a dirty working tree and a rejected push, write the diagnostic commands you would run before changing anything.

## Mini-project submission

Include:
1. A small project structure and `.gitignore`.
2. At least five logical commits.
3. One feature branch and merge.
4. A short history review using `git log`.
5. A recovery demonstration in a disposable repository.
6. A note explaining what should not be committed (secrets, private data, large artifacts).

## Self-review

- [ ] I checked status and diffs before staging/committing.
- [ ] Commit messages describe intent.
- [ ] I understand local commits are not automatically pushed to a remote.
- [ ] I can distinguish fetch, pull, and push.
- [ ] I have not committed secrets or unapproved data.
- [ ] I know which undo commands are destructive.

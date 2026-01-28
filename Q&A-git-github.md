# Git & GitHub Interview Questions (DevOps / Fresher to Mid-Level)

This README contains **50 Git & GitHub interview questions with clear, medium-length answers**, tailored for **DevOps, fresher, and mid-level roles**. All content is preserved exactly as provided, structured properly for readability and interview preparation.

---

## Git Interview Questions (1–20)

### 1. What is Git?

Git is a distributed version control system used to track changes in source code. It allows multiple developers to work simultaneously, maintain history, and roll back to previous versions when needed.

### 2. Difference between Git and GitHub

Git is a version control tool that runs locally, while GitHub is a cloud-based platform that hosts Git repositories and provides collaboration features like pull requests and issue tracking.

### 3. What is a repository in Git?

A repository (repo) is a directory that contains project files along with Git metadata. It stores the entire history of changes and can be local or remote.

### 4. What is a commit?

A commit is a snapshot of changes made to files at a specific point in time. Each commit has a unique hash, author details, and a message describing the change.

### 5. What is the Git workflow?

A typical Git workflow includes:

* Working Directory (edit files)
* Staging Area (git add)
* Repository (git commit)
* Push changes to remote (git push)

### 6. What is the staging area?

The staging area is an intermediate space where changes are reviewed before committing. It allows selective commits instead of committing all modified files.

### 7. What is git clone?

git clone creates a local copy of a remote repository, including all branches and commit history.

### 8. What is git pull vs git fetch?

* git fetch downloads changes without merging
* git pull downloads and merges changes into the current branch

### 9. What is a branch in Git?

A branch is an independent line of development. It allows developers to work on features or fixes without affecting the main codebase.

### 10. What is the default branch in Git?

Traditionally it was master, but now main is the default branch in most modern repositories.

### 11. What is git merge?

git merge integrates changes from one branch into another. It creates a merge commit unless fast-forward merging is possible.

### 12. What is a merge conflict?

A merge conflict occurs when Git cannot automatically resolve differences between branches. The developer must manually fix conflicting lines.

### 13. What is git rebase?

git rebase moves or reapplies commits onto another base commit. It creates a cleaner, linear commit history but rewrites commit hashes.

### 14. Difference between merge and rebase

* Merge preserves history and creates a merge commit
* Rebase rewrites history and creates a linear timeline

### 15. What is git stash?

git stash temporarily saves uncommitted changes, allowing you to switch branches without committing incomplete work.

### 16. What is .gitignore?

.gitignore specifies files or directories that Git should not track, such as logs, environment files, and build artifacts.

### 17. What is git reset?

git reset is used to undo commits or unstage files. It has three modes:

* --soft
* --mixed
* --hard

### 18. What is git revert?

git revert creates a new commit that reverses the changes of a previous commit, making it safer for shared branches.

### 19. What is a detached HEAD?

A detached HEAD state occurs when you checkout a specific commit instead of a branch, meaning new commits won’t belong to any branch.

### 20. What is git cherry-pick?

git cherry-pick applies a specific commit from one branch to another without merging the entire branch.

---

## GitHub Interview Questions (21–30)

### 21. What is GitHub?

GitHub is a web-based platform that hosts Git repositories and supports collaboration, version control, CI/CD, and project management.

### 22. What is a pull request (PR)?

A pull request is a request to merge changes from one branch into another. It enables code review, discussion, and approval.

### 23. What is a fork in GitHub?

A fork is a personal copy of someone else’s repository. It allows you to experiment or contribute without affecting the original project.

### 24. Difference between fork and clone

* Fork creates a copy on GitHub
* Clone creates a local copy on your machine

### 25. What are GitHub issues?

Issues are used to track bugs, enhancements, tasks, and discussions related to a repository.

### 26. What are GitHub Actions?

GitHub Actions is a CI/CD tool that automates workflows such as building, testing, and deploying applications on events like push or PR.

### 27. What is a webhook in GitHub?

A webhook sends HTTP requests to external services when specific events occur, such as push or pull request creation.

### 28. What is a protected branch?

A protected branch restricts direct commits and requires pull requests, reviews, or passing CI checks before merging.

### 29. What is semantic versioning?

Semantic versioning follows MAJOR.MINOR.PATCH format to indicate breaking changes, new features, and bug fixes.

### 30. How is Git & GitHub used in DevOps?

Git manages source code, while GitHub enables collaboration and automation through CI/CD pipelines, code reviews, and infrastructure versioning.

---

## Git Interview Questions (31–42)

### 31. What is HEAD in Git?

HEAD is a pointer that refers to the current branch or commit you are working on. When you make a commit, it moves forward to the latest commit.

### 32. What is origin in Git?

origin is the default name given to the remote repository when a project is cloned. It represents the remote source of the code.

### 33. What is git remote?

git remote is used to manage remote repositories. You can add, remove, rename, or view remotes using commands like git remote add and git remote -v.

### 34. What is git log used for?

git log displays the commit history of a repository, showing commit IDs, authors, dates, and messages. It helps in auditing and debugging changes.

### 35. What is git diff?

git diff shows the differences between commits, branches, or the working directory and staging area. It is useful for reviewing changes before committing.

### 36. What is git tag?

git tag is used to mark specific points in history, usually for releases like v1.0.0. Tags are immutable references to commits.

### 37. What are lightweight and annotated tags?

* Lightweight tags are simple pointers to commits
* Annotated tags store metadata like tagger name, date, and message

### 38. What is a fast-forward merge?

A fast-forward merge occurs when the target branch has no new commits. Git simply moves the branch pointer forward without creating a merge commit.

### 39. What is git blame?

git blame shows who last modified each line of a file, along with the commit hash. It is useful for tracing bugs or understanding code ownership.

### 40. What is git clean?

git clean removes untracked files from the working directory. It is commonly used to clean up build artifacts or temporary files.

### 41. What is git bisect?

git bisect helps find the commit that introduced a bug by performing a binary search between known good and bad commits.

### 42. What is git reflog?

git reflog records updates to branch references. It allows recovery of commits even after reset or rebase.

---

## GitHub Interview Questions (43–50)

### 43. What is GitHub Flow?

GitHub Flow is a lightweight branching strategy where developers create feature branches, open pull requests, review code, and merge into main.

### 44. What is a GitHub release?

A GitHub release is a packaged version of software tied to a Git tag. It may include release notes and compiled binaries.

### 45. What is CODEOWNERS file?

The CODEOWNERS file defines individuals or teams responsible for specific parts of the codebase. It automates review requests.

### 46. What is a GitHub organization?

A GitHub organization allows teams to manage repositories, permissions, and workflows under a single entity.

### 47. What are GitHub secrets?

GitHub secrets securely store sensitive values like API keys and passwords used in GitHub Actions workflows.

### 48. What is Dependabot?

Dependabot automatically checks dependencies for vulnerabilities and creates pull requests to update them.

### 49. What is the difference between GitHub Actions and Jenkins?

GitHub Actions is GitHub-native and event-driven, while Jenkins is a standalone CI/CD tool requiring separate setup and maintenance.

### 50. How do you resolve conflicts in a pull request?

Conflicts are resolved by pulling the latest changes, manually fixing conflicting files, staging them, and pushing the resolved commit.

---

✅ **Use this README.md for:**

* DevOps interview preparation
* Git & GitHub revision
* GitHub repository documentation
* Fresher to mid-level role interviews

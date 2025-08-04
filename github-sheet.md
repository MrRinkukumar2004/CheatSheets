# Git Command Cheat Sheet with Examples

---

## 1. **Git Configuration**

| Command                          | Description                | Example                                             |
| -------------------------------- | -------------------------- | --------------------------------------------------- |
| `git config --global user.name`  | Set global username        | `git config --global user.name "John Doe"`          |
| `git config --global user.email` | Set global email           | `git config --global user.email "john@example.com"` |
| `git config --list`              | List current configuration | `git config --list`                                 |

---

## 2. **Repository Initialization**

| Command           | Description                   | Example                                      |
| ----------------- | ----------------------------- | -------------------------------------------- |
| `git init`        | Initialize new Git repository | `git init`                                   |
| `git clone <url>` | Clone remote repo locally     | `git clone https://github.com/user/repo.git` |

---

## 3. **Checking Status**

| Command             | Description                     | Example             |
| ------------------- | ------------------------------- | ------------------- |
| `git status`        | Show working directory status   | `git status`        |
| `git log`           | Show commit history             | `git log`           |
| `git log --oneline` | Show commit history in one line | `git log --oneline` |
| `git diff`          | Show unstaged changes           | `git diff`          |
| `git diff --staged` | Show staged changes             | `git diff --staged` |

---

## 4. **Staging and Committing**

| Command                        | Description                            | Example                                 |
| ------------------------------ | -------------------------------------- | --------------------------------------- |
| `git add <file>`               | Stage a file for commit                | `git add index.js`                      |
| `git add .`                    | Stage all changes in current directory | `git add .`                             |
| `git commit -m "<message>"`    | Commit staged changes with message     | `git commit -m "Fix bug in login flow"` |
| `git commit -a -m "<message>"` | Commit tracked files directly          | `git commit -a -m "Update README file"` |
| `git reset <file>`             | Unstage a file                         | `git reset index.js`                    |
| `git rm --cached <file>`       | Unstage and keep file locally          | `git rm --cached secret.txt`            |

---

## 5. **Branching**

| Command                         | Description              | Example                          |
| ------------------------------- | ------------------------ | -------------------------------- |
| `git branch`                    | List all branches        | `git branch`                     |
| `git branch <branch_name>`      | Create new branch        | `git branch feature/login`       |
| `git checkout <branch_name>`    | Switch branch            | `git checkout develop`           |
| `git checkout -b <branch_name>` | Create and switch branch | `git checkout -b feature/signup` |
| `git branch -d <branch_name>`   | Delete a branch          | `git branch -d feature/login`    |

---

## 6. **Merging**

| Command                      | Description                                | Example                     |
| ---------------------------- | ------------------------------------------ | --------------------------- |
| `git merge <branch_name>`    | Merge specified branch into current        | `git merge feature/login`   |
| `git merge --no-ff <branch>` | Merge with commit always (no fast-forward) | `git merge --no-ff develop` |

---

## 7. **Remote Repositories**

| Command                       | Description                    | Example                                                  |
| ----------------------------- | ------------------------------ | -------------------------------------------------------- |
| `git remote -v`               | List remote repos              | `git remote -v`                                          |
| `git remote add <name> <url>` | Add remote repo                | `git remote add origin https://github.com/user/repo.git` |
| `git fetch <remote>`          | Fetch remote branch info       | `git fetch origin`                                       |
| `git pull <remote> <branch>`  | Fetch + merge remote branch    | `git pull origin main`                                   |
| `git push <remote> <branch>`  | Push local branch to remote    | `git push origin feature/login`                          |
| `git push -u origin <branch>` | Push and set upstream tracking | `git push -u origin feature/signup`                      |

---

## 8. **Undoing Changes**

| Command                          | Description                               | Example                    |
| -------------------------------- | ----------------------------------------- | -------------------------- |
| `git checkout -- <file>`         | Discard changes in working directory      | `git checkout -- index.js` |
| `git reset HEAD <file>`          | Unstage a file                            | `git reset HEAD index.js`  |
| `git revert <commit_hash>`       | Create new commit that reverts a previous | `git revert 1a2b3c4d`      |
| `git reset --soft <commit_hash>` | Reset commit pointer, keep changes staged | `git reset --soft HEAD~1`  |
| `git reset --hard <commit_hash>` | Reset commit pointer, discard changes     | `git reset --hard HEAD~1`  |

---

## 9. **Stashing**

| Command           | Description                  | Example                    |
| ----------------- | ---------------------------- | -------------------------- |
| `git stash`       | Save changes temporarily     | `git stash`                |
| `git stash list`  | List stashes                 | `git stash list`           |
| `git stash pop`   | Apply and remove stash       | `git stash pop`            |
| `git stash apply` | Apply stash without removing | `git stash apply`          |
| `git stash drop`  | Delete a stash               | `git stash drop stash@{0}` |

---

## 10. **Tagging**

| Command                   | Description          | Example                                        |
| ------------------------- | -------------------- | ---------------------------------------------- |
| `git tag`                 | List tags            | `git tag`                                      |
| `git tag <tag_name>`      | Create tag           | `git tag v1.0.0`                               |
| `git tag -a <tag> -m ""`  | Create annotated tag | `git tag -a v1.0.0 -m "Release version 1.0.0"` |
| `git push <remote> <tag>` | Push tag             | `git push origin v1.0.0`                       |

---

## 11. **Viewing History**

| Command             | Description              | Example                     |
| ------------------- | ------------------------ | --------------------------- |
| `git log`           | Show commit logs         | `git log`                   |
| `git log --oneline` | One line per commit      | `git log --oneline`         |
| `git log --graph`   | Show branching graph     | `git log --graph --oneline` |
| `git show <commit>` | Show changes of a commit | `git show HEAD`             |

---

## 12. **Checking Differences**

| Command                        | Description                      | Example                |
| ------------------------------ | -------------------------------- | ---------------------- |
| `git diff`                     | Show unstaged changes            | `git diff`             |
| `git diff --staged`            | Show staged changes              | `git diff --staged`    |
| `git diff <commit1> <commit2>` | Show differences between commits | `git diff HEAD~1 HEAD` |

---

## Tips

- Keep commit messages clear and concise:  
  **Format:** `<type>(<scope>): <subject>`  
  Example: `feat(auth): add JWT token validation middleware`.
- Stage related changes together for atomic commits.
- Frequently pull changes before pushing to avoid conflicts.

---

This cheat sheet covers the essential Git commands you’ll use daily with examples to help you get started and maintain clean version control.

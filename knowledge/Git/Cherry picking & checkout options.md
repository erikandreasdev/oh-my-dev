### **Cherry-Picking in Git**
Cherry-picking allows you to select specific commits from one branch and apply them to another without merging the entire branch history.

#### Key Commands:
1. **Single Commit Cherry-Pick:**
   ```bash
   git cherry-pick <commit-hash>
   ```
   - Applies a single commit to the current branch.

2. **Range of Commits:**
   ```bash
   git cherry-pick <start-commit-hash>...<end-commit-hash>
   ```
   - Selectively applies a series of commits.

#### Example Workflow:
1. **Move Commit `r` from Hotfix to Feature Branch:**
   ```bash
   git switch feature
   git cherry-pick r
   ```

#### Use Cases:
- **Bug Fixes:** Quickly apply fixes to stable branches without merging unrelated changes.
- **Feature Integration:** Move only relevant changes from feature branches to the main branch.
- **Backporting:** Apply changes from newer branches to older versions.
- **Experimentation:** Test specific commits in different branches before full integration.

#### **Drawback**:
Cherry-picking can create duplicate commits across branches, potentially cluttering commit history. Consider merging if branch integrity isn’t an issue.

---

### **Git Checkout: Managing Branches, Commits, and Files**
The `git checkout` command enables branch switching, file restoration, and branch creation from commits. It modifies your working directory to match a specified branch or commit.

#### Key Commands:
1. **Switching Branches:**
   ```bash
   git checkout <branch-name>
   ```
   - Example: Move from `master` to `hotfix` for quick fixes.
   
2. **Retrieve Specific File Versions:**
   ```bash
   git checkout <commit-hash> -- <file-path>
   ```
   - Example: Retrieve `script.js` from a past commit.
   ```bash
   git checkout ca82a6dff817ec66f44342007202690a93763949 -- src/buttons/script.js
   ```

3. **Create New Branches from Specific Commits:**
   ```bash
   git checkout -b <new-branch-name> <starting-point>
   ```
   - Example: Create `feature-branch` from `master`:
   ```bash
   git checkout -b feature-branch master
   ```

#### Use Cases:
- **Branch Switching:** Quickly switch between branches for different tasks.
- **Inspecting Commit States:** View repository state at any commit.
- **File Recovery:** Restore specific files from previous commits.
- **New Branch Creation:** Create branches from specific history points for hotfixes or feature testing.

---

### **Summary of Commands:**
- **`git cherry-pick <commit-hash>`**: Apply specific commits to the current branch.
- **`git checkout <branch-name>`**: Switch between branches.
- **`git checkout <commit-hash> -- <file-path>`**: Retrieve previous file versions.
- **`git checkout -b <new-branch-name> <starting-point>`**: Create a new branch from an existing commit.

---

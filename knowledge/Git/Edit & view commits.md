### **Monitoring Commits**

#### Key Commands:
1. **View Commit History**:
   ```bash
   git log
   ```
   - Lists all commits with details such as commit ID, author, date, and message.

2. **Show Commit Details**:
   ```bash
   git show <commit-hash>
   ```
   - Displays details of a specific commit, including changes made.

3. **Compare Commits**:
   ```bash
   git diff <commit-hash1>..<commit-hash2>
   ```
   - Shows the differences between two commits, making it easy to track changes.

---

### **Modifying Commits**

#### Key Commands:
1. **Amend Last Commit**:
   ```bash
   git commit --amend
   ```
   - Use this immediately after a commit to fix the message or add files. This command rewrites the last commit.

2. **Revert an Older Commit**:
   ```bash
   git revert <commit-hash>
   ```
   - Creates a new commit that undoes changes from a specific commit. Use `git revert HEAD` to undo the most recent commit.  

> **Note:** Reverting may lead to conflicts if the file has newer changes.

---

### **Canceling Commits with Checkout**

#### Key Commands:
1. **Retrieve File from Specific Commit**:
   ```bash
   git checkout <commit-hash> -- <file-path>
   ```
   - Restores a file to its state in a particular commit without affecting other changes. 

---

### **Summary of Commands**

1. **Monitoring**:
   - `git log`: Lists commits with their identifiers.
   - `git show <commit-hash>`: Shows details of a specific commit.
   - `git diff <commit1>..<commit2>`: Compares changes between commits.

2. **Modifying**:
   - `git commit --amend`: Corrects the last commit’s message or adds files.
   - `git revert <commit-hash>`: Reverts an older commit, preserving history.

3. **Canceling**:
   - `git checkout <commit-hash> -- <file-path>`: Retrieves a file from a specific commit to fix changes without affecting the overall commit history.

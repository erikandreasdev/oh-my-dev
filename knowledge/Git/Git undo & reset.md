
1. **Git Reset**:
   - **Soft Reset**: Reverts the last commit while preserving staged changes and working tree.
     ```bash
     git reset --soft @~1
     ```
   - **Mixed Reset** (default behavior): Resets to a specified commit, clearing the staging area but keeping changes in the working tree.
     ```bash
     git reset --mixed @~1
     ```
   - **Hard Reset**: Resets to a specified commit, clearing the staging area and removing changes from the working tree. **Use carefully** as this will delete uncommitted changes permanently.
     ```bash
     git reset --hard @~1
     ```

2. **Unstage Files from the Staging Area**:
   - Use `git reset @ filename` to remove a specific file from the staging area.
     ```bash
     git reset @ file-2
     ```
   - Alternatively, use `git restore`:
     ```bash
     git restore --staged file-2
     ```

3. **Restore Modified Files**:
   - Use `git restore` to revert a file in the working tree to the last committed state.
     ```bash
     git restore file-1
     ```
   - Or use `git checkout`:
     ```bash
     git checkout -- file-1
     ```

4. **Git Revert for Remote Commits**:
   - To undo a commit that was pushed to the remote repository, use `git revert` instead of `git reset` to avoid creating an inconsistent history.
     ```bash
     git revert @
     git push origin main
     ```

5. **Undo a Hard Reset**:
   - If you lose committed changes due to `git reset --hard`, use `git reflog` to view commit history and recover lost commits.
     ```bash
     git reflog
     ```

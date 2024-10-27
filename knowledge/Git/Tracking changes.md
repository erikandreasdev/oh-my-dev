### **Finding Who Changed What with Git Blame**

The `git blame` command reveals which commit last modified each line in a file and who made the change. This is essential for tracking down the source of an error or understanding code history.

#### Key Commands:
1. **Identify Author and Commit for Each Line**:
   ```bash
   git blame <file>
   ```
   - Example:
     ```bash
     git blame code.py
     ```
   - Shows the author, date, and commit hash for each line in `code.py`.

---

### **Exploring File Changes with Git Log**

The `git log` command is versatile for viewing commit history and investigating how specific files were modified.

#### Key Commands:
1. **View Commit History of a File**:
   ```bash
   git log <file>
   ```
   - Shows all commits that modified the specified file.

2. **View Details of the Last Change to a File**:
   ```bash
   git log -n1 <file>
   ```
   - Displays only the most recent commit for the file.

3. **Detailed View of a Specific Commit**:
   ```bash
   git show --name-only <commit-hash>
   ```
   - Example:
     ```bash
     git show --name-only ^36a1d7
     ```
   - Shows files modified in a specific commit, along with the commit message.

4. **Discover Additional Log Options**:
   ```bash
   git log --help
   ```
   - Lists various options, like filtering by date, author, and commit message keywords.

---

### **Searching by Keyword in Commits**

Git allows you to search for specific keywords or phrases within commits. This is useful when you’re tracking down a set of changes by keyword, phrase, or a particular task description.

#### Key Commands:
1. **Find Commits by Code Content**:
   ```bash
   git log -G <keyword>
   ```
   - Example:
     ```bash
     git log -G updated
     ```
   - Lists all commits where the specified keyword appears in the code.

2. **Find Commits by Message Content**:
   ```bash
   git log --all --grep='<phrase>'
   ```
   - Example:
     ```bash
     git log --all --grep='README'
     ```
   - Shows all commits with a message that includes the specified phrase.

---

### **Summary of Commands for Tracking Changes and Contributions**

- **Identify Last Change in File**:
  - `git blame <file>`
- **View All Commits Modifying a File**:
  - `git log <file>`
- **Find Commits with Specific Keywords in Code**:
  - `git log -G <keyword>`
- **Find Commits with Keywords in Description**:
  - `git log --all --grep='<phrase>'`

## Introduction

In Unix, file permissions control who can read, write, or execute files. Understanding and managing these permissions is essential for maintaining system security and sharing files correctly. This guide covers how to view and modify file permissions, using both numeric (octal) and symbolic modes.

---

## Top Web References

- **[GNU Coreutils Documentation for `ls`](https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html)**: Details on using `ls` and its options.
- **[chmod Command Manual](https://man7.org/linux/man-pages/man1/chmod.1.html)**: Comprehensive guide to using `chmod` for permissions.
- **[chown Command Manual](https://man7.org/linux/man-pages/man1/chown.1.html)**: Explains `chown` command options for changing file ownership.

---

## Key Concepts with Real-World Examples

### 1. Viewing File Permissions

**Command**: `ls -l` and `stat`

- **Concept**: Use `ls -l` to view permissions and details (owner, group, size) of files in a directory. `stat` provides additional metadata, including permissions in octal format.
  
- **Example**:
  ```bash
  $ ls -l file.txt
  -rw-r--r-- 1 admin admin 0 Mar 12 16:22 file.txt
  ```

  This output shows:
  - `-rw-r--r--`: Permissions for owner (`rw`), group (`r`), and others (`r`).
  - `admin admin`: Owner and group.

### 2. Changing File Ownership

**Command**: `chown`

- **Concept**: Use `chown` to change the owner and/or group of a file or directory. This is useful for assigning file ownership to specific users.
  
- **Example**:
  ```bash
  $ chown superman:marvel ./save_the_world.txt
  ```
  This command changes the owner to `superman` and the group to `marvel` for `save_the_world.txt`.

- **Recursive Example**:
  ```bash
  $ chown -R superman:marvel ./my_directory
  ```
  The `-R` option applies the ownership change to all files and subdirectories.

### 3. Changing File Permissions with `chmod`

#### Absolute Mode (Octal Notation)

**Command**: `chmod [permissions] [file]`

- **Concept**: Use octal numbers to set permissions for the owner, group, and others. This approach is efficient for setting all permissions at once.

- **Example**:
  ```bash
  $ chmod 751 modify_it_now.exe
  ```
  Breakdown of `751`:
  - `7` (Owner): Read (`4`) + Write (`2`) + Execute (`1`)
  - `5` (Group): Read (`4`) + Execute (`1`)
  - `1` (Others): Execute only

#### Symbolic Mode (Using Operators)

**Command**: `chmod [user] [operator] [permissions] [file]`

- **Concept**: Use symbolic mode to adjust permissions selectively with operators:
  - `+` adds permission
  - `-` removes permission
  - `=` sets permission, replacing existing settings

- **Example**:
  ```bash
  # Allow owner to read, write, and execute
  $ chmod u=rwx modify_it_now.exe

  # Allow group to read and execute
  $ chmod g=rx modify_it_now.exe

  # Allow others only to read
  $ chmod o=r modify_it_now.exe
  ```

- **Additional Example**:
  ```bash
  # Grant read permission to all users without altering other permissions
  $ chmod a+r file.txt
  ```

### 4. Summary of Permission Symbols

- **Permission Characters**:
  - `r` - Read
  - `w` - Write
  - `x` - Execute
- **User Denotations**:
  - `u` - User (Owner)
  - `g` - Group
  - `o` - Others
  - `a` - All (user, group, and others)

### 5. Real-World Scenario Example

Imagine you’re setting up a shared directory (`/project_docs`) for a team where:
- The owner should have full permissions.
- The group should only read and edit files.
- Others should not have any access.

Commands to configure this:
```bash
# Change ownership to admin and group to team
$ chown admin:team /project_docs

# Set permissions: owner has rwx, group has rw, others have none
$ chmod 760 /project_docs
```

---

## Summary of Concepts

- **View Permissions**: Use `ls -l` for file listing and permissions or `stat` for octal and detailed metadata.
- **Change Owner**: `chown user:group file` changes the file owner and/or group.
- **Modify Permissions**: Use `chmod` with either octal (absolute) or symbolic modes.
  - **Absolute Mode**: Set permissions in one step with octal codes (e.g., `chmod 755 file`).
  - **Symbolic Mode**: Use symbols to modify specific permissions (e.g., `chmod u+w file`).

---

## Cheatsheet

| **Command**       | **Description**                                                  |
|-------------------|------------------------------------------------------------------|
| `ls -l [file]`    | View file permissions and details                               |
| `stat [file]`     | View detailed file metadata, including permissions in octal     |
| `chown user:group [file]` | Change file owner and group                           |
| `chmod [permissions] [file]` | Change file permissions (octal or symbolic mode)     |
| **Permissions Symbols** | `r` (Read), `w` (Write), `x` (Execute)                  |
| **User Denotations**     | `u` (User), `g` (Group), `o` (Others), `a` (All)       |
| **Operators**            | `+` (Add), `-` (Remove), `=` (Set)                    |

https://www.conventionalcommits.org/en/v1.0.0/
### 🎯 **Conventional Commits Cheatsheet**

#### Basic Format

A commit message should follow this structure:

```plaintext
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

#### **Commit Message Components**

1. **`type`**: The purpose of the commit.
2. **`scope`** (optional): Part of the codebase the commit affects.
3. **`description`**: A short message in imperative mood explaining what the commit does.
4. **`body`** (optional): A more detailed explanation of the change.
5. **`footer(s)`** (optional): Extra information, including **BREAKING CHANGES** or **issue references**.

---

#### 📋 **Types**

| Type           | Purpose                                      | Example                                    |
|----------------|----------------------------------------------|--------------------------------------------|
| `feat`         | New feature                                  | `feat(parser): add support for new syntax` |
| `fix`          | Bug fix                                      | `fix(api): handle null values in response` |
| `docs`         | Documentation only changes                   | `docs(readme): update installation guide`  |
| `style`        | Code style changes (whitespace, formatting)  | `style(css): reformat buttons layout`      |
| `refactor`     | Code change that neither fixes a bug nor adds a feature | `refactor(user): simplify auth flow`       |
| `perf`         | Performance improvements                     | `perf(core): optimize data caching`        |
| `test`         | Adding or fixing tests                       | `test(auth): add login tests`              |
| `build`        | Changes affecting the build system or dependencies | `build(webpack): upgrade webpack version` |
| `ci`           | Changes to CI configuration or scripts       | `ci(github-actions): add build status badge` |
| `chore`        | Minor tasks or updates (e.g., refactoring)   | `chore(deps): bump library versions`       |
| `revert`       | Reverting a previous commit                  | `revert: remove incorrect user validation` |

---

#### 📌 **Scopes**

*Define scopes to describe what section of the codebase the commit affects.* Examples include `auth`, `parser`, `database`, `ui`, `cli`, `tests`, etc.

```plaintext
feat(auth): add multi-factor authentication
```

---

#### 📜 **Descriptions**

- Write in the **imperative mood** (e.g., "add", "remove", "fix").
- Keep it **short** and **specific**.

---

#### 📖 **Body** (Optional)

Use the **body** to provide additional context about the change.

```plaintext
fix(ui): align button text

Button text was slightly off-center due to incorrect padding.
Fixed by adjusting the CSS properties.
```

---

#### ⚠️ **Footers** (Optional)

##### **Breaking Changes**

If your commit introduces a breaking change, include it in the footer with `BREAKING CHANGE: <explanation>`.

```plaintext
feat(api): update user endpoint to return profile

BREAKING CHANGE: The /user endpoint now returns a user profile object instead of a string.
```

##### **Referencing Issues**

If your commit closes, references, or affects an issue, include it in the footer:

```plaintext
fix(auth): handle expired tokens

Closes #123
```

---

#### 🚀 **Example Commit Messages**

- **Feature**: `feat(ui): add dark mode toggle`
- **Bug Fix**: `fix(database): correct typo in schema migration`
- **Refactor**: `refactor(cli): simplify argument parsing`
- **Breaking Change**:
  ```plaintext
  feat(auth): replace token storage method

  BREAKING CHANGE: Tokens are now stored in IndexedDB instead of localStorage.
  ```

#### 🛠️ **Tips for Writing Good Conventional Commits**

1. **Consistency**: Follow the same structure for every commit.
2. **Conciseness**: Keep messages brief but informative.
3. **Context**: Use scope effectively to identify areas affected.
4. **Separate Logical Changes**: Each commit should do one thing.

---

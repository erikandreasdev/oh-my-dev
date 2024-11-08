The `on` directive in GitHub Actions allows for fine-grained control over when and how workflows are triggered, making it a powerful tool for automating processes, orchestrating workflows, and integrating external services.

Below, we’ll explore key event-based triggers that let you maximize GitHub Actions' potential for CI/CD pipelines.

---

### **1. Manual Launch of Workflows with `workflow_dispatch`**

The `workflow_dispatch` event enables workflows to be manually triggered, providing flexibility to run a workflow without making any code changes. You can also pass custom parameters during the manual trigger.

#### Example Workflow
```yaml
name: Manual trigger
on:
  workflow_dispatch:
    inputs:
      name:
        description: "Whom to greet"
        default: "World"

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Hello Step 
        run: echo "Hello ${{ github.event.inputs.name }}"
```

- **How it works**: When a user navigates to the **Actions** tab, they can select this workflow and click **Run workflow**, passing a value for the `name` parameter if desired.
- **Real-world Use Case**: Developers use `workflow_dispatch` to manually trigger workflows for testing or special deployments without needing to push changes.

---

### **2. Creating Workflow Dependencies with `workflow_run`**

The `workflow_run` event triggers a workflow based on the status of another workflow, allowing you to create dependencies and sequences across workflows.

#### Example Scenario
You have two workflows, `build.yml` and `test.yml`. You want `test.yml` to run only when `build.yml` succeeds.

**build.yml**
```yaml
name: Build
on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Step
        run: echo "Build is running"
```

**test.yml**
```yaml
name: Test
on:
  workflow_run:
    workflows: [Build]
    types: [completed]

jobs:
  on-success:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    steps:
      - name: Success Message
        run: echo 'Build workflow passed'

  on-failure:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    steps:
      - name: Failure Message
        run: echo 'Build workflow failed'
```

- **How it works**: `test.yml` triggers when `build.yml` completes. The jobs are conditional, running based on whether the build succeeded or failed.
- **Real-world Use Case**: QA teams often use `workflow_run` to run tests only after a successful build workflow.

---

### **3. Running Reusable Workflows with `workflow_call`**

The `workflow_call` event allows workflows to invoke other workflows, enabling modular and reusable workflow design within or across repositories.

#### Example Scenario
Imagine you have a main workflow (`ping.yml`) that calls a reusable workflow (`pong.yml`) to handle a repetitive task, promoting code reusability.

**ping.yml**
```yaml
name: Workflow A (Ping)
on:
  push:
    branches:
      - main

jobs:
  ping:
    uses: USER_OR_ORG_NAME/REPO_NAME/.github/workflows/pong.yaml@master
    with:
      ping: 'PONG'
```

**pong.yml**
```yaml
name: Workflow B (Pong)
on:
  workflow_call:
    inputs:
      ping:
        required: true
        type: string

jobs:
  pong:
    runs-on: ubuntu-latest
    steps:
      - name: Pong Response
        run: echo "${{ inputs.ping }}"
```

- **How it works**: The `ping.yml` workflow invokes `pong.yml`, passing the parameter `ping` with the value `"PONG"`.
- **Real-world Use Case**: DevOps teams create reusable workflows for common tasks (e.g., security checks, notifications) that can be invoked from various workflows to reduce redundancy.

---

### **4. Triggering Cross-Repository Workflows with `repository_dispatch`**

The `repository_dispatch` event allows you to trigger workflows across repositories using the GitHub REST API. This can enable complex automation that spans multiple repositories.

#### Example Scenario
Here, a workflow in Repository B sends a dispatch request to Repository A, initiating a workflow based on an external event.

**Repository A Workflow (Responder)**
```yaml
name: Remote Dispatch Responder
on: repository_dispatch

jobs:
  ping-pong:
    runs-on: ubuntu-latest
    steps:
      - name: Event Information
        run: |
          echo "Event '${{ github.event.action }}' received from '${{ github.event.client_payload.repository }}'"
```

**Repository B Workflow (Dispatcher)**
```yaml
name: Remote Dispatch Action
on:
  push:

jobs:
  ping-pong:
    runs-on: ubuntu-latest
    steps:
      - name: Send Dispatch
        run: |
          curl -L \
            -X POST \
            -H "Accept: application/vnd.github+json" \
            -H "Authorization: token ${{secrets.ACCESS_TOKEN}}" \
            -H "X-GitHub-Api-Version: 2022-11-28" \
            https://api.github.com/repos/OWNER/REPO/dispatches \
            -d '{"event_type": "ping", "client_payload": { "repository": "'"$GITHUB_REPOSITORY"'" }}'
```

- **How it works**: The workflow in Repository B uses the GitHub API to send a `repository_dispatch` event to Repository A, triggering the responder workflow there.
- **Real-world Use Case**: Organizations automate multi-repository pipelines where each repository handles a stage of CI/CD and notifies others on completion.

---

### **Summary of Concepts**

- **`workflow_dispatch`**: Enables manual workflow triggers with customizable inputs.
- **`workflow_run`**: Creates workflow dependencies, triggering workflows based on others' completion status.
- **`workflow_call`**: Allows for reusable, modular workflows that can be invoked within or across repositories.
- **`repository_dispatch`**: Enables triggering workflows across repositories, ideal for complex, multi-repository automation.

---

### **Cheatsheet**

- **`workflow_dispatch`**
  - **Purpose**: Manually trigger workflows.
  - **Syntax**: `on: workflow_dispatch`
  - **Use Case**: Ad-hoc testing or deployments.
  
- **`workflow_run`**
  - **Purpose**: Trigger workflows based on another workflow’s completion.
  - **Syntax**: `on: workflow_run` with `workflows` and `types` options.
  - **Use Case**: Sequential workflow execution.

- **`workflow_call`**
  - **Purpose**: Reuse workflows.
  - **Syntax**: `on: workflow_call`
  - **Use Case**: Shared logic for consistency across workflows.

- **`repository_dispatch`**
  - **Purpose**: Trigger workflows in other repositories.
  - **Syntax**: `on: repository_dispatch`
  - **Use Case**: Multi-repository pipeline orchestration.


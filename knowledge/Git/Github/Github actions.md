### Components of GitHub Actions

|Component|Description|Example|
|---|---|---|
|**Workflow**|A collection of jobs that define an automated process. Workflows are stored in `.github/workflows/`.|`name: CI Workflow`|
|**Event**|Triggers that initiate workflows, such as `push`, `pull_request`, or scheduled events.|`on: push`|
|**Job**|A set of steps that runs on a specific runner. Multiple jobs can run in parallel or in sequence.|`jobs: build`|
|**Step**|A single task within a job, consisting of commands or actions.|`- name: Run Tests`|
|**Action**|A reusable command or set of commands that accomplishes a task, can be used from GitHub Marketplace.|`uses: actions/checkout@v2`|
|**Runner**|A server that runs the workflows, can be GitHub-hosted (Ubuntu, Windows, macOS) or self-hosted.|`runs-on: ubuntu-latest`|
|**Secrets**|Secure environment variables stored in GitHub for sensitive data like API keys and credentials.|`${{ secrets.MY_SECRET_KEY }}`|
|**Environment**|Configurations that hold variables for specific environments like `production` or `staging`.|`environment: production`|
|**Matrix**|Allows jobs to run on multiple versions or configurations, creating parallel job runs.|`strategy: matrix: [node: 12, 14]`|
|**Artifacts**|Files created by workflows (like build outputs) that can be downloaded or used in other jobs.|`- name: Upload Artifact`|
|**Cache**|Reusable data (like dependencies) that speeds up subsequent workflow runs by caching files.|`uses: actions/cache@v2`|
|**Permissions**|Defines specific repository permissions for workflow actions, enhancing security.|`permissions: contents: read`|
|**Workflow Dispatch**|A manual trigger that lets users run workflows manually from the GitHub Actions UI.|`on: workflow_dispatch`|
|**Reusable Workflow**|A workflow designed to be called by other workflows, promoting reusability across repositories.|`workflow_call: from: another-repo/.github/actions/`|--

---
### Workflow Syntax Cheat Sheet

- **Workflow YAML Location**: Place YAML files in `.github/workflows/`
- **Basic Syntax**:

```yaml
# .github/workflows/basic-workflow.yml

name: Basic Workflow  # Optional - names the workflow

on: push  # Event that triggers the workflow; "push" is one of the simplest

jobs:
  example-job:  # Name of the job
    runs-on: ubuntu-latest  # Runner (environment) for the job
    
    steps:  # Steps define the tasks in the job
      - name: Checkout code  # Step 1: Checks out the repository code
        uses: actions/checkout@v2

      - name: Run a script  # Step 2: Runs a simple script
        run: echo "Hello, GitHub Actions!"

```

---

### Event Types and Triggers
|Event|Description|
|---|---|
|`push`|Triggered on a push to a branch or tag.|
|`pull_request`|Triggered when a pull request is opened, updated, or closed.|
|`pull_request_target`|Triggered on pull requests from a fork targeting the base repository. Useful for security-sensitive workflows.|
|`create`|Triggered when a branch or tag is created.|
|`delete`|Triggered when a branch or tag is deleted.|
|`workflow_dispatch`|Triggered manually from the GitHub Actions UI.|
|`repository_dispatch`|Triggered by an external system using a GitHub API request.|
|`schedule`|Triggered at a specified time using cron syntax.|
|`issue_comment`|Triggered when a comment is created, edited, or deleted in an issue.|
|`issues`|Triggered when an issue is created, edited, labeled, or closed.|
|`label`|Triggered when a label is created, edited, or deleted.|
|`fork`|Triggered when someone forks the repository.|
|`star`|Triggered when someone stars the repository.|
|`watch`|Triggered when someone watches the repository.|
|`release`|Triggered when a release is created, edited, published, unpublished, or deleted.|
|`deployment`|Triggered when a deployment is created.|
|`deployment_status`|Triggered when the deployment status changes (e.g., success, failure).|
|`status`|Triggered when the status of a GitHub commit changes.|
|`check_run`|Triggered when a check run is created, updated, or completed.|
|`check_suite`|Triggered when a check suite is created, requested, or completed.|
|`page_build`|Triggered when a GitHub Pages site is built or deployed.|
|`public`|Triggered when a private repository is made public.|
|`workflow_run`|Triggered when a workflow run is completed and allows subsequent workflows to be triggered conditionally.|
|`gollum`|Triggered when a Wiki page is created or updated.|
|`project`|Triggered when a project board is created, updated, or deleted.|
|`project_card`|Triggered when a project card is created, moved, or deleted.|
|`project_column`|Triggered when a project column is created, updated, or deleted.|
|`repository`|Triggered when a repository is created, deleted, archived, unarchived, or renamed.|
|`member`|Triggered when a collaborator is added to or removed from the repository.|
|`workflow_call`|Enables reusable workflows that can be triggered by other workflows within the repository.|

---

### Defining Jobs

Jobs define the tasks to be executed within a workflow.

#### Basic Job Definition

```yaml
jobs:
  build:
    runs-on: ubuntu-latest  # Runner environment
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run a script
        run: echo "Hello from the build job!"
```

#### Matrix Strategy (Multiple OS/Versions)

Runs the job on multiple OS and version combinations.

```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        node: [12, 14]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node }}
      - run: node -v
```

---

### Commonly Used Actions

| Action                        | Description                         | Example                             |
| ----------------------------- | ----------------------------------- | ----------------------------------- |
| `actions/checkout@v2`         | Checks out code from the repository | `uses: actions/checkout@v2`         |
| `actions/setup-node@v3`       | Sets up Node.js environment         | `uses: actions/setup-node@v3`       |
| `actions/cache@v2`            | Caches dependencies                 | `uses: actions/cache@v2`            |
| `docker/build-push-action@v2` | Builds and pushes Docker images     | `uses: docker/build-push-action@v2` |
Full list -> https://github.com/marketplace

---

### Using Secrets and Environment Variables

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Access Secret
        env:
          MY_SECRET: ${{ secrets.MY_SECRET }}
        run: echo "The secret is $MY_SECRET"
```

---

### Example Workflow with Multiple Jobs

This example demonstrates a CI workflow that tests code on different Node.js versions and deploys it on a successful build.

```yaml
name: CI Workflow

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12, 14, 16]
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  deploy:
    needs: test
    if: success()
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy Application
        env:
          API_KEY: ${{ secrets.DEPLOY_API_KEY }}
        run: ./deploy.sh
```

For a Spring project, GitHub Actions workflows can automate testing, building, and deploying your application. Below, I’ll cover workflows specific to Spring Boot applications that involve common tasks: running tests, building a Docker image, and deploying to cloud services.

---

### 1. Basic Spring Boot CI Workflow

This workflow runs on every `push` or `pull request` event, checks out the code, sets up Java, and runs tests.

```yaml
name: CI - Build and Test

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'  # Set to your Java version

      - name: Cache Maven packages
        uses: actions/cache@v2
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
          restore-keys: |
            ${{ runner.os }}-maven-

      - name: Build with Maven
        run: mvn clean package

      - name: Run tests
        run: mvn test
```

---

### 2. Build and Push Docker Image to Docker Hub

This workflow builds a Docker image of the Spring Boot application and pushes it to Docker Hub on every push to `main`.

```yaml
name: Docker Build and Push

on:
  push:
    branches:
      - main

jobs:
  docker-build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v2
        with:
          context: .
          file: Dockerfile
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/spring-boot-app:latest
```

---

### 3. Deploy to Heroku

This workflow deploys the Spring Boot application to Heroku after successful build and tests. Replace `HEROKU_API_KEY` with your secret and `HEROKU_APP_NAME` with your app’s name.

```yaml
name: Deploy to Heroku

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'

      - name: Install Heroku CLI
        run: |
          curl https://cli-assets.heroku.com/install.sh | sh

      - name: Authenticate with Heroku
        env:
          HEROKU_API_KEY: ${{ secrets.HEROKU_API_KEY }}
        run: heroku auth:token

      - name: Deploy to Heroku
        run: |
          heroku git:remote -a ${{ secrets.HEROKU_APP_NAME }}
          git push heroku main
```

---

### 4. Build and Deploy to AWS Elastic Beanstalk

For AWS Elastic Beanstalk, we first build the application and then deploy it using the AWS CLI.

```yaml
name: Deploy to AWS Elastic Beanstalk

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'

      - name: Build with Maven
        run: mvn clean package -DskipTests

      - name: Upload JAR to S3
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws s3 cp target/my-spring-app.jar s3://my-spring-bucket/my-spring-app.jar

      - name: Deploy to Elastic Beanstalk
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws elasticbeanstalk create-application-version --application-name MySpringApp \
            --version-label v1 --source-bundle S3Bucket="my-spring-bucket",S3Key="my-spring-app.jar"
          aws elasticbeanstalk update-environment --environment-name MySpringApp-env \
            --version-label v1
```

---

### 5. Scheduled Spring Boot Application Health Check

This scheduled workflow checks if the Spring Boot application is running by sending an HTTP request to a given URL.

```yaml
name: Health Check

on:
  schedule:
    - cron: '0 0 * * *'  # Runs daily at midnight

jobs:
  health-check:
    runs-on: ubuntu-latest

    steps:
      - name: Perform Health Check
        run: |
          response=$(curl --write-out '%{http_code}' --silent --output /dev/null https://my-spring-app.com/health)
          if [ $response -ne 200 ]; then
            echo "Health check failed!"
            exit 1
          else
            echo "Health check passed!"
          fi
```

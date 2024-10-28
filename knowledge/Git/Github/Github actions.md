### Components of GitHub Actions

| Component  | Description                                                                                                                                                                      | Example                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| **Events** | Triggers that start workflows. Examples include `push`, `pull_request`, or a scheduled `cron`.                                                                                  | `on: push`                   |
| **Workflows** | Composed of jobs, stored in `.github/workflows`, triggered by an event.                                                                                                   | `.github/workflows/build.yml`|
| **Jobs**    | A series of steps that run in the same runner environment, can be parallel or sequential.                                                                                       | `jobs: build`                |
| **Steps**   | Individual tasks inside a job, including commands and actions.                                                                                                                  | `steps: - name: Build step`  |
| **Actions** | Reusable tasks from GitHub Marketplace or custom ones.                                                                                                                         | `uses: actions/checkout@v2`  |
| **Runners** | Virtual machines (hosted by GitHub or self-hosted) where jobs execute.                                                                                                          | `runs-on: ubuntu-latest`     |

---

### Workflow Syntax Cheat Sheet

- **Workflow YAML Location**: Place YAML files in `.github/workflows/`
- **Basic Syntax**:

  ```yaml
  name: Workflow Name
  on: push  # Triggered by push event

  jobs:
    job_name:
      runs-on: ubuntu-latest  # Runner type
      steps:
        - name: Step Description
          run: echo "Hello, GitHub Actions!"
  ```

---

### Event Types and Triggers

| Event       | Description                                  | Example                     |
|-------------|----------------------------------------------|-----------------------------|
| `push`      | Triggered on code push                       | `on: push`                  |
| `pull_request` | Triggered on PR open/update/close        | `on: pull_request`          |
| `schedule`  | Triggered at specific intervals (cron)       | `on: schedule`              |
| `workflow_dispatch` | Manual trigger from the GitHub UI   | `on: workflow_dispatch`     |
| `repository_dispatch` | External events using webhooks    | `on: repository_dispatch`   |

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

| Action                    | Description                                     | Example                                 |
|---------------------------|-------------------------------------------------|-----------------------------------------|
| `actions/checkout@v2`     | Checks out code from the repository             | `uses: actions/checkout@v2`             |
| `actions/setup-node@v3`   | Sets up Node.js environment                     | `uses: actions/setup-node@v3`           |
| `actions/cache@v2`        | Caches dependencies                             | `uses: actions/cache@v2`                |
| `docker/build-push-action@v2` | Builds and pushes Docker images         | `uses: docker/build-push-action@v2`     |

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

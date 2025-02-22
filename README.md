# RepoPipeline

# DevOps Engineer Homework

## How did you test your pipelines?
Before executing the pipeline, I use Jenkins Pipeline Linter (pipeline-linter) or jenkinsfile-runner to validate the syntax of the Jenkinsfile.
This helps in catching syntax errors early before running the pipeline.
2. Unit Testing & Static Code Analysis:
I ensure unit tests run as part of the pipeline using JUnit, TestNG, or Mockito.
I also integrate SonarQube or other static analysis tools to check for code quality issues.

3. Pipeline Execution Testing:
I validate each stage (e.g., checkout, build, test, deploy) using mock builds in a sandbox environment before deploying to production.
I use Blue Ocean or Jenkins logs to track execution flow and debug failures.

4. Error Handling & Failure Scenarios:
I test failure scenarios using intentional failures (e.g., breaking a test, removing dependencies) to ensure the pipeline behaves correctly.
I verify that retry mechanisms and fail-fast strategies are in place to handle transient issues.

5. Security & Compliance Checks:
I ensure secrets and credentials are securely managed via Jenkins credentials store and not exposed in logs.
I integrate SAST/DAST tools (e.g., Snyk, OWASP Dependency Check) for security scans in the pipeline.

6. Performance & Load Testing:
I use JMeter or Gatling for performance testing to analyze how the application behaves under load.
I ensure pipeline execution time is optimized by caching dependencies and parallel execution.

7. Notifications & Reporting:
I validate that Slack, email, or Teams notifications are triggered correctly on failures and successes.
I ensure that JUnit test reports and build artifacts are published correctly in Jenkins.


## How did you test RepoC Python?
- Manually ran `log_parser.py warnings.log output.csv`.
- Verified output.

## Why use Git LFS?
- Git LFS (Large File Storage) is used to manage large binary files efficiently in Git repositories. Instead of storing large files directly in the repo, Git LFS replaces them with lightweight pointers and stores the actual files separately.

## How to enable Git LFS?
1. Install LFS: `git lfs install`
2. Track files: `git lfs track "*.bin"`
3. Commit and push:
   ```sh or Bat
   git add .gitattributes
   git add <binary_file>
   git commit -m "Enable LFS"
   git push

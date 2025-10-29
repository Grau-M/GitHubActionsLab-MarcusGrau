# Lab #4: GitHub Actions Lab

This repository contains three GitHub Actions workflows demonstrating key CI/CD concepts.

## Workflows

### 1. `dependent-jobs.yml`
* **Purpose:** This workflow demonstrates job dependencies.
* **Key Concepts:** It uses the `needs` key to force jobs to run in a specific order: `build` -> `test` -> `deploy`.

### 2. `env-and-secrets.yml`
* **Purpose:** This workflow demonstrates the use of environment variables at different scopes and how to use repository secrets.
* **Key Concepts:**
    * `on: workflow_dispatch`: Allows manual triggering.
    * `env:`: Used at the workflow, job, and step levels to show variable scope.
    * `secrets`: Shows how to access `secrets.AWS_ACCESS_KEY_ID` in a secure way (they are masked in logs).

### 3. `multi-platform.yml`
* **Purpose:** This workflow demonstrates parallel, multi-platform testing.
* **Key Concepts:**
    * `on: pull_request`: Triggers the workflow when a PR is opened.
    * `runs-on`: Uses `ubuntu-latest`, `windows-latest`, and `macos-latest`.
    * **Parallel Execution:** Because no `needs` key is present, all three jobs run simultaneously.

## Challenges Faced

The most significant challenge was the strictness and pickiness of the YAML syntax. My `env-and-secrets.yml` workflow repeatedly failed with "Invalid workflow file" errors, even though the code looked correct.

I discovered two separate issues:

1.  **Improper Indentation:** In one instance, a `run:` key was not indented to the same level as its corresponding `name:` key, which broke the step's structure.
2.  **Invisible Characters:** The more difficult problem was that my file contained non-breaking space characters instead of standard spaces for indentation. The YAML parser does not recognize these, causing the file to be invalid.

To solve these issues, I carefully read the GitHub Actions error logs to find the exact line number of the failure. I then meticulously deleted all the leading whitespace (invisible spaces) for the problematic lines and re-indented them manually using the spacebar.

After several failed commits, the repository history was messy. To fix this, I used `git reset --hard` to revert my local repository to the first working commit, then used `git push --force` to overwrite the remote repository. This gave me a clean slate to commit the fully corrected files, which then executed successfully.
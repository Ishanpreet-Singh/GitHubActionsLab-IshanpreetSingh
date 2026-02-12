# GitHub Actions Lab (Job Dependencies + Multi-Platform)

## Repository Goal
This lab demonstrates two GitHub Actions workflows:
1. **Workflow 1 (Dependent Jobs)**: runs jobs in order using job dependencies.
2. **Workflow 2 (Multi-Platform Testing)**: runs jobs in parallel on different operating systems.

---

## Workflow 1: Dependent Jobs (build → test → deploy)
**File:** `.github/workflows/dependent-jobs.yml`  
**Trigger:** `push` to the `main` branch

### Purpose
To show how GitHub Actions can enforce a required execution order:
- Build must finish before Test
- Test must finish before Deploy

### Key Concepts Demonstrated
- **needs:** creates job dependencies and forces order  
  Example:
  - `test` needs `build`
  - `deploy` needs `test`
- **runs-on:** selects the runner OS (example: `ubuntu-latest`)
- **env:** can define environment variables (workflow/job/step level).  
  (In this lab, env can be used to store values like APP_NAME or VERSION.)

---

## Workflow 2: Multi-Platform Testing (Parallel Jobs)
**File:** `.github/workflows/multi-platform.yml`  
**Trigger:** `pull_request` targeting the `main` branch

### Purpose
To show how GitHub Actions can run independent jobs **at the same time** on:
- Ubuntu
- Windows
- macOS

Each job:
- checks out the repository using `actions/checkout@v4`
- prints OS information
- runs an OS-specific command (Linux/macOS: `uname -a`, Windows: `systeminfo`)
- creates a small file and prints its contents

### Key Concepts Demonstrated
- **runs-on:** different runners (`ubuntu-latest`, `windows-latest`, `macos-latest`)
- **Parallel execution:** no `needs` is used, so jobs run simultaneously
- **actions/checkout@v4:** downloads the repo code into the runner
- **env:** can be used to store shared values (example: build version), and the runner provides built-in variables like `RUNNER_OS`

---

## How I Added and Pushed the Workflows (Steps I Followed)

### Part A (Workflow 1) - Push to main
1. Created workflow folder: `.github/workflows/`
2. Added `dependent-jobs.yml` into that folder
3. Committed changes (IDE Git Commit)
4. Pushed to GitHub (Push to remote origin)
5. Verified in GitHub → **Actions tab** that jobs ran in the correct order:
   **build → test → deploy**

### Part B (Workflow 2) - Pull Request to main
1. Created a new branch for testing (example: `test-multi-platform`)
2. Added `multi-platform.yml` into `.github/workflows/`
3. Committed changes on the new branch
4. Pushed branch to GitHub
5. Opened GitHub → Pull Requests → created a PR from `test-multi-platform` → `main`
6. Verified in GitHub PR **Checks / Actions tab** that:
   - Ubuntu job
   - Windows job
   - macOS job  
   all ran **in parallel**
7. Merged the pull request into `main`

---

## Challenges Faced & How I Solved Them
1. **Workflow showed “no runs yet”**
   - Cause: workflow only triggers on `push` or `pull_request`, and it wasn’t triggered yet.
   - Fix: pushed a new commit to `main` (Workflow 1) and created a PR (Workflow 2).

2. **Different commands on Windows vs Linux/macOS**
   - Cause: `uname -a` does not work the same on Windows runners.
   - Fix: used Windows PowerShell commands like `systeminfo` and `Get-Content`.

3. **Hidden .github folder in IDE**
   - Cause: some IDE views hide dot-folders.
   - Fix: refreshed the project and disabled filters that hide dot files.

---

## Screenshots Included (Submission)
- Workflow file contents (both YAML files)
- Actions tab showing workflows
- Workflow 1: dependency visualization (build → test → deploy)
- Workflow 2: all 3 OS jobs running/ran successfully in parallel
- Logs showing successful output (OS info + file content)

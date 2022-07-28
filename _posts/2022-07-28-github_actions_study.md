---
layout: single
title:  "What is GitHub Actions?"
categories: CI_CD
tags:
  - ci/cd
  - devops
  - github actions
toc: true
toc_sticky: true
---

💡 CI/CD Tool Study

## GitHub Actions

GitHub Actions is a continuous integratoin and continuous delivery (CI/CD) platform that allows to automate the build, test, and deployment pipeline.

### The components of GitHub Actions

* **Workflows**: A configurable *automated process* that will run one or more jobs.
  * YAML file.
  * Can be triggered by an event in the repository, or manually, or at a defined schedule.
  * Defined in the `.github/workflows` directory in a repository.

   
* **Events**: A specific activity that *triggers a workflow* run.
  * Examples:
    * A *pull request* is created
    * An *issue* is open
    * A *commit* is pushed
    * Run on a *schedule*
    * Posting to a *REST API*
    * Manually

  
* **Jobs**: A set of *steps* that execute on the same runner.
  * Each step is either a shell script, or an action that will be run.
  * Steps are executed in order and are dependent on each other.
  * Steps share data from one to another.
    * Example:
      * Step_1 - builds an application.
      * Step_2 - tests the application.
  * Jobs run in parallel and have no dependencies by default.
    * If a job takes a dependency on another job, it will wait for the dependent job to complete before it can run.

  
* **Actions**: A *custom application* for the GitHub Actions platform that performs a complex but frequently repeated task.
  * Help reduce the repetitive code in the workflow files.
  * Example:
    * Pull a git repository,
    * Set up the correct toolchain for the build environment,
    * Set up the authentication to the cloud provider.

  
* **Runners**: A server that runs the workflows when they're triggered.
  * Ubuntu Linux, Microsoft Windows, and macOS runners in a newly-provisioned virtual machine.
  * Hosting the user own runners is possible.


## Example workflow

```yaml
name: learn-github-actions
on: [push]
jobs:
  check-bats-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: npm install -g bats
      - run: bats -v
```

* `name: learn-github-actions` - The name of the workflow that will appear in the Actions tab.
* `on: [push]` - Specifies the trigger for this workflow.
* `jobs:` - Group of the jobs in the workflow.
* `check-bats-version:` - The name of a job.
* `runs-on: ubuntu-latest` - Configuration. Run on the latest version of an Ubuntu Linux runner.
* `steps:` - Group of the steps in the job.
* `- uses: actions/checkout@v3` - Run `v3` of the `actions/checkout` action.
* `- run: npm install -g bats` - Execute a command on the runner.

  
(Reference: GitHub Docs [https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions))
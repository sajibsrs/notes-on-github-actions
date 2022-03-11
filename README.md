# Notes on GitHub Actions
GitHub actions is an implementation of a cohesive automated **continuous integration, continuous delivery (CI/CD)** workflow.

## # Contents
  - [1 # Workflows](#1--workflows)
  - [2 # Events](#2--events)
  - [3 # Jobs](#3--jobs)
  - [4 # Steps](#4--steps)
  - [5 # Actions](#5--actions)
  - [6 # Runners](#6--runners)
  - [License](#license)

## 1 # Workflows
A workflow is a configurable automated process that that runs a single or multiple jobs. Workflows are defined in a **YAML** file. GitHub checks the repository for workflow configuration file and runs when triggered by an event in the repository. Workflows can also be triggered manually or on a predefined schedule.

One repository can have multiple workflows for different operations. Such as, a repository can have one workflow to build and test pull requests and another to deploy application every time a release is created.

*Note: Workflows can be reused rather than duplicating existing one.*

* [Reusing workflow *[ext]*](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)
* [Learn about workflow *[ext]*](https://docs.github.com/en/actions/using-workflows)

*Note: *[ext]* in a link denotes, an external link that leads to other location outside of this document.*

## 2 # Events
An event is a specific activity in a repository that triggers a workflow run. For example, activity can originate from GitHub on creating pull request, opening issue, push, or committing to a repository. Workflow run can be triggered in **three** ways:

1. Schedule
2. Posting to a REST API
3. Manually

[GitHub events *[ext]*](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

## 3 # Jobs
A job is a set of steps that run on the same runner. Multiple jobs within the same workflow can run sequentially, although by default they runs in parallel.

```yml
jobs:
  tests_manual_workflow:
    runs-on: ubuntu-latest
```

## 4 # Steps
Steps are individual tasks that runs commands, such as a shell command or an action, in a job within a workflow. Steps can share data among themselves as they runs on the same runner.

```yml
steps:
  - run: >
    echo "User ${{ github.event.inputs.username }} ran a workflow."
    echo "Reason ${{ github.event.inputs.reason }}."
```
## 5 # Actions
Actions are standalone commands. They combined into steps to create **job**. User can create their own actions and share with the community. User can also use actions that have already been created by the community.

```yml
jobs:
  stale:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/stale@v3
```
## 6 # Runners
A runner is a server application, often installed on a virtual machine or docker container. It runs a job from GitHub Actions workflow. A runner has one job at a time and provides feedback to GitHub. In a runner, steps executes in the same instance of the virtual machine (**VM**). This allows the actions within that job to share information using the filesystem.

There are two types of runners:
1. GitHub-hosted runners
2. Self-hosted runners

*Note: GitHub supports Windows, macOS, and Linux operating systems for runners.*

```yml
jobs:
  build:
    runs-on: macos-latest
```
GitHub hosted runner is managed by GitHub. On the other hand, self-hosted runners are created and managed by users.

*Note: There are three types of actions: **Docker container actions, JavaScript actions, and composite run steps actions**.*

## # License
[MIT](LICENSE)

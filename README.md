# Core Concepts of GitHub Actions
GitHub actions is an implementation of a cohesive automated **continuous integration, continuous delivery and continuous deployment (CI/CD)** workflow.

## 1 # Components
* [Events](#2--events)
* [Jobs](#3--jobs)
* [Steps](#4--steps)
* [Actions](#5--actions)
* [Runners](#6--runners)

## 2 # Events
GitHub actions are event-driven. You can define what happens when a specific event occurs. Event, works as an input in GitHub Action.
[More about GitHub events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#available-events).

Workflows can be triggered by three groups of events:
1. [Scheduled events](#21--scheduled-events)
2. [Manual events](#22--manual-events)
3. [Webhook events](#23--webhook-events)

### 2.1 # Scheduled events
Scheduled events triggers on a specified time. It uses **POSIX** cron syntax. 

*The minimum time can be set to 5 minutes.*

```yml
on:
  schedule:
    - cron: '*/5 * * * *'
```
*Note: For **POSIX** cron syntax help visit [crontab.guru](https://crontab.guru).*

### 2.2 # Manual events
Although the most popular and convenient way to use GitHub Actions is to run automated workflow. It's also possible to run those workflows manually.

There are two different types of manual events:
1. workflow_dispatch
2. repository_dispatch

#### 2.2.1 # workflow_dispatch
To manually trigger a workflow, use the **workflow_dispatch** event. You can manually trigger a workflow run using the GitHub API, GitHub CLI, or GitHub browser interface. For more information see [Manually running workflow](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow).

Following example requires input from the user and prints the user's input to the logs:

```yml
on:
  workflow_dispatch:
    inputs:
      username:
        description: 'Your user name'
        required: true
      reason:
        description: 'Why this workflow is being run manually?'
        required: true
        default: 'On testing purpose'
```

#### 2.2.2 # repository_dispatch
*Note: This event will only trigger a workflow run if the workflow file is on the default branch.*

You can use the GitHub API to trigger a webhook event called **repository_dispatch** when you want to trigger a workflow for activity that happens outside of GitHub. For more information, see [Create a repository dispatch event](https://docs.github.com/en/rest/reference/repos#create-a-repository-dispatch-event).

Any data that you send through the client_payload parameter will be available in the github.event context in your workflow. 

For example, if you send this request body when you create a repository dispatch event:

```json
{
  "event_type": "test_result",
  "client_payload": {
    "passed": false,
    "message": "Error: timeout"
  }
}
```

then you can access the payload in a workflow like this:

```yml
on:
  repository_dispatch:
    types: [test_result]

jobs:
  run_if_failure:
    if: ${{ !github.event.client_payload.passed }}
    runs-on: ubuntu-latest
    steps:
      - env:
          MESSAGE: ${{ github.event.client_payload.message }}
        run: echo $MESSAGE
```

### 2.3 # Webhook events
These events triggers on workflow when GitHub webhook event. Such as issue and pull request, update and deletion, deployment etc.

Each event corresponds to a certain set of actions that can happen to your organization and/or repository. For example, if you subscribe to the issues event you'll receive detailed payloads every time an issue is opened, closed, labeled, etc.

Although the **issues** event has over a dozen types that could trigger a workflow, this can be narrowed down by setting **type**.

```yml
on:
  issues:
    types: [opened]
```
This one will only trigger on if the issue type is opened. [Learn more about GitHub webhooks](https://docs.github.com/en/developers/webhooks-and-events/webhooks/creating-webhooks).

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

## License
[MIT](LICENSE)

# Core Concepts of GitHub Actions
GitHub actions is an implementation of a cohesive automated **continuous integration, continuous delivery and continuous deployment (CI/CD)** workflow.

## 1 # Components
1. Events
2. Jobs
3. Steps
4. Actions
5. Runners

## 2 # Events
GitHub actions are event-driven. You can define what happens when a specific event occurs. Event, works as an input in GitHub Action.

Workflows can be triggered by three groups of events:
1. Scheduled events
2. Manual events
3. Webhook events

### 2.1 # Scheduled events
Scheduled events triggers on a specified time. It uses **POSIX** cron syntax. 

*The minimum time can be set to 5 minutes.*

```yml
on:
  schedule:
    - cron: '*/5 * * * *'
```
*Note: For **POSIX** cron syntax help visit [here](https://crontab.guru).*

### 2.2 # Manual events
Although the most popular and convenient way to use GitHub Actions is to run automated workflow. It's also possible to run those workflows manually.

There are two different types of manual events:
1. workflow_dispatch
2. repository_dispatch

#### 2.2.1 workflow_dispatch
The workflow_dispatch event can be used to trigger specific workflows within a repository manually. It allows defining *custom, default, and required input properties* within the workflow file. These inputs can be accessed using `github.event.inputs` context.

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
*Note: Workflow must be on the default branch to trigger `workflow_dispatch`.*

### 2.3 # Webhook events
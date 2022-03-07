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
For **POSIX** cron syntax help [visit](https://crontab.guru).

### 2.2 # Manual events
### 2.3 # Webhook events
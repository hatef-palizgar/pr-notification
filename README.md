# PR Notification 

[![PR Notifications](https://github.com/hatef-palizgar/pr-notification-poc/actions/workflows/pr-notification.yml/badge.svg)](https://github.com/hatef-palizgar/pr-notification-poc/actions/workflows/pr-notification.yml)

A GitHub Action that automatically extracts service information from pull request titles and notifies relevant Slack channels based on the affected services.

## Features

- Extracts service names from PR titles
- Maps services to notification Slack channels using a configurable mapping file
- Extracts JIRA ticket numbers from PR titles
- Supports multiple services in a single PR
- Provides debug information for easier troubleshooting
- Automatically sends notifications to configured Slack channels

## How It Works

1. The action is triggered when a pull request is created or updated
2. It extracts service names from the PR title (content within parentheses)
3. Using the `service-mapping.json` file, it maps each service to its corresponding notification Slack channel
4. If present, it also extracts and includes the JIRA ticket number (format: QRND-XXXX) in the notification
5. The workflow then sends notifications to the identified Slack channels using the Slack GitHub Action

## Prerequisites

- A Slack Bot Token with appropriate permissions (stored as `SLACK_BOT_TOKEN` in repository secrets)
- Properly configured Slack channels that match the mapping in `service-mapping.json`

## Example PR Title Format

```
feat(service1, service2, QRND-1234): add new feature
```

## Contributing

Feel free to open issues or submit pull requests if you have suggestions for improvements.

# PR Notification 🔔

[![PR Notifications](https://github.com/hatef-palizgar/pr-notification-poc/actions/workflows/pr-notification.yml/badge.svg)](https://github.com/hatef-palizgar/pr-notification-poc/actions/workflows/pr-notification.yml)

A GitHub Action that automatically extracts service information from pull request titles and notifies relevant Slack channels based on the affected services.

## Features ✨

- Extracts service names from PR titles
- Maps services to notification Slack channels using a configurable mapping file
- Extracts JIRA ticket numbers from PR titles
- Supports multiple services in a single PR
- Provides debug information for easier troubleshooting
- Automatically sends notifications to configured Slack channels

## How It Works 🛠️

1. The action is triggered when a pull request is created or updated
2. It extracts service names from the PR title (content within parentheses)
3. Using the `service-mapping.yaml` file, it maps each service to its corresponding notification Slack channel
4. If present, it also extracts and includes the JIRA ticket number (format: QRND-XXXX) in the notification
5. The workflow then sends notifications to the identified Slack channels using the Slack GitHub Action

## Prerequisites ⚡

- A Slack Bot Token with appropriate permissions (stored as `SLACK_BOT_TOKEN` in repository secrets)
- Properly configured Slack channels that match the mapping in `service-mapping.yaml`

## Repository Visibility Configuration 🔒

### For Private Repositories
If you're using this action in a private repository, you'll need to:
1. Create a Personal Access Token (PAT) with `repo` scope:
   - Go to GitHub.com → Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Generate a new token with `repo` scope
   - Copy the token value
2. Add the token as a repository secret:
   - Go to your repository → Settings → Secrets and variables → Actions
   - Create a new secret named `QUINICORN_ACTIONS` [^1]
   - Paste your token value

[^1]: Check with your DevOps team for the correct secret name and if they are already configured.

### For Public Repositories
If your repository is public, no additional configuration is needed. The workflow will work out of the box.

## Example PR Title Format 📝

```
feat(service1, service2, QRND-1234): add new feature
```

## Local Testing with Act 🧪

To test this GitHub Action locally, you can use the `act` tool. Here's how but for learning the act tool, check the [official documentation](https://github.com/nektos/act):

1. Install act (MacOS):
```bash
brew install act
```

2. List all workflows in the repository:
```bash
act -l --container-architecture linux/amd64
```

3. Create a pull request event simulation file `event.json`:
```json
{
  "pull_request": {
    "number": 123,
    "user": {
      "login": "hatef-palizgar"
    },
    "title": "feat(time-punch, QRND-4568): add new feature",
    "body": "This pull request introduces a new feature that significantly improves the performance of data processing tasks. Key changes include optimized algorithms and refined data structures.",
    "html_url": "https://github.com/hatef-palizgar/PR/123",
    "head": {
      "ref": "feature"
    },
    "base": {
      "ref": "main"
    }
  }
}
```

4. Run the action with your Slack token and event file:
```bash
act pull_request -s SLACK_BOT_TOKEN=<your-slack-token> --container-architecture linux/amd64 -e event.json
```

## Usage 📋

### 1. Service Mapping Configuration

Create or modify the `service-mapping.yaml` file in `.github/actions/extract-pr-data/` directory:

```yaml
# Map your services to Slack channels
service-name: slack-channel-name
another-service: another-channel
```

### 2. PR Title Format

The action expects PR titles to follow this format:
```
type(service1, service2, QRND-XXXX): description
```

Where:
- `type`: The type of change (e.g., feat, fix, chore)
- `service1, service2`: One or more service names in parentheses (must match names in service-mapping.yaml)
- `QRND-XXXX`: Optional JIRA ticket number
- `description`: Brief description of the changes

Examples:
```
feat(meal-break, QRND-1234): add new feature
fix(overtime-methods): fix bug in calculation
chore(salaries, time-punch): update dependencies
```

### 3. Workflow Setup

1. Add the workflow file to `.github/workflows/pr-notification.yml`
2. Configure required secrets:
   - `SLACK_BOT_TOKEN`: Your Slack Bot User OAuth Token
   - `QUINICORN_ACTIONS`: Personal/Organisation GitHub PAT with `repo` scope

### 4. Slack Channel Setup

1. Create the necessary Slack channels that match your service-mapping.yaml
2. Ensure the bot has necessary permissions:
   - `chat:write`
   - `chat:write.public`

## Notification Format 📬

The Slack notification includes:
- PR Title
- PR Number
- Repository Name
- Author
- JIRA Ticket (if present) with link
- `View Pull Request` button


## Troubleshooting 🔧

### Common Issues

1. **No Notification Sent**
   - Check if service names in PR title match service-mapping.yaml
   - Verify Slack bot token permissions
   - Ensure bot scope includes `chat:write.public`

2. **Invalid Channel Error**
   - Verify channel names in service-mapping.yaml exist in Slack
   - Check if bot has access to the channels

3. **Service Not Recognized**
   - Ensure service names in PR title exactly match service-mapping.yaml
   - Check for typos or case mismatches

## Contributing 🤝
Feel free to open issues or submit pull requests if you have suggestions for improvements.

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

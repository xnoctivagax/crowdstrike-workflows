# CrowdStrike Falcon Fusion Workflows

Sanitised examples of Falcon Fusion workflows I've built. Each folder is one workflow with the YAML and a notes file explaining what it does.

## Workflows

- [slack-detection-alerts](./slack-detection-alerts) — posts every EPP detection to Slack with formatted context
- (more to come)

## Rebuilding these elsewhere

The YAML isn't drop-in importable. Tenant-specific IDs and credentials need replacing. See each workflow's notes.md for the details.

Placeholders to replace when rebuilding:
- `SLACK_CHANNEL_ID_HERE`
- `SLACK_CONFIG_ID_HERE`
- `ACTION_ID_PLACEHOLDER`

# Slack v2 Integration Setup for Fusion

Setup steps for connecting Slack to CrowdStrike Falcon Fusion. Has to be done once per tenant before any Slack-using workflow can run. This is the finicky part, not the workflows themselves.

## 1. Create a Slack app

- Go to [Slack API: Your Apps](https://api.slack.com/apps)
- Click **Create New App**
- Choose **Create from scratch** or **Create from manifest**
- Name the app Crowdstrike and pick the target workspace
- Click **Create App**

## 2. Get the app credentials

- In the new app, go to **Basic Information**
- Copy the **Client ID** and **Client Secret** from the App Credentials section

## 3. Configure OAuth

- Go to **OAuth & Permissions** in the left sidebar
- Under OAuth Tokens & Redirect URLs, add the Redirect URI for your CrowdStrike region:

| Region | Redirect URI |
|--------|--------------|
| US 1 | `https://api.crowdstrike.com/webhooks/53189b3998004bcb95973739717b8291/v1` |
| US 2 | `https://api.us-2.crowdstrike.com/webhooks/95ff048d66304364a421b0fd0d1bbcd4/v1` |
| EU | `https://api.eu-1.crowdstrike.com/webhooks/5c1d08398c894f998eee6f0e66639411/v1` |
| Gov-1 | `https://api.laggar.gcw.crowdstrike.com/webhooks/5c1d08398c894f998eee6f0e66639411/v1` |
| Gov-2 | `https://api.us-gov-2.crowdstrike.mil/webhooks/5c1d08398c894f998eee6f0e66639411/v1` |

- Click **Opt In** for token rotation (recommended)

## 4. Configure in Fusion SOAR

- In Fusion SOAR, go to the Slack v2 app listing → **Configure**
- Fill in:
  - **Name:** something recognisable, e.g. `Slack V2 Integration`
  - **Base URL:** `https://slack.com`
  - **Client ID:** from step 2
  - **Client Secret:** from step 2

- For Falcon Identity Protection use case, select **Identity Protection** in the Use case field, then enable these scopes:
  - `users:read.email`
  - `users.read`
  - `chat.write`

- Click **Save**

## 5. Verify

- Confirm the config saved successfully
- Run a test action to verify connectivity

## Notes

- The Redirect URIs above were correct at the time I set this up. CrowdStrike may update them, so check their current docs if any of these stop working.
- You only do this setup once per Falcon tenant per Slack workspace. After that, every workflow that uses Slack references this saved configuration via its `config_id`.
- The `config_id` in the workflow YAML files (`SLACK_CONFIG_ID_HERE` placeholder) refers to the configuration created in step 4 and is not something that needs to be filled in.

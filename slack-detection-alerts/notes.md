# Slack Detection Alerts

Posts every Falcon EPP detection to Slack with formatted context.

## Flow

1. EPP detection trigger (no filtering, fires on every detection)
2. Get Device Details 
3. Slack v2 chat.postMessage with Block Kit formatting

## Slack message contents

Hostname, severity, local IP, process name, SHA256, description, and a deep link to the detection in Falcon.

## Sanitisation

- `SLACK_CONFIG_ID_HERE` — Slack integration config ID
- `SLACK_CHANNEL_ID_HERE` — target channel
- `ACTION_ID_PLACEHOLDER` — internal action instance IDs (Fusion regenerates these on import)

## Rebuild

1. Set up Slack v2 integration in Falcon, note the config ID
2. Create new workflow, trigger: Detection → EPP Detection
3. Add Get Device Details action, device_id: `${Trigger.Detection.EPP.Sensor.SensorID}`
4. Add Slack v2 chat.postMessage action with the JSON from the YAML
5. Replace placeholders with real values
6. Test with a controlled detection (EICAR or similar)

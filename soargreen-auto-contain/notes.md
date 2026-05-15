# SOARGreen Auto-Containment

Auto-contains hosts on Critical EPP detections, tag-gated by `FalconGroupingTags/SOARGreen`.

## Flow

1. EPP detection trigger
2. Condition: severity = Critical AND host has SOARGreen tag
3. Get Device Details
4. Contain Device
5. Slack message to alert channel

## Condition expression

`Trigger.Detection.Severity:5+Trigger.Detection.EPP.Sensor.Tags:['FalconGroupingTags/SOARGreen']`

Severity 5 = Critical in CrowdStrike's scale. The tag check uses the full `FalconGroupingTags/SOARGreen` path.

## Slack message

Block Kit formatted. Includes hostname, severity, local IP, process, SHA256, description, and a deep link to the detection in Falcon. Says "auto-containment triggered" and notes the SOARGreen tag matched.

## Sanitisation

- `SLACK_CONFIG_ID_HERE` — Slack integration config ID
- `SLACK_CHANNEL_ID_HERE` — target channel
- `ACTION_ID_PLACEHOLDER` — internal action instance IDs

## Rebuild

1. Slack v2 integration set up in Falcon (see top-level README)
2. New workflow, trigger: Detection → EPP Detection
3. Add condition: severity is Critical AND sensor host tags includes `FalconGroupingTags/SOARGreen`
4. Add Get Device Details action, device_id: `${Trigger.Detection.EPP.Sensor.SensorID}`
5. Add Contain Device action, same device_id
6. Add Slack v2 chat.postMessage with Block Kit JSON from the YAML
7. Replace placeholders with real values
8. Tag a test host with `SOARGreen`, fire a controlled detection (EICAR or similar). Note: EICAR usually fires as low severity, you may need a Critical-severity test detection or temporarily lower the threshold to test.


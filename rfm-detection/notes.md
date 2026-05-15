# RFM Detection Alert

Posts a Slack alert when a host enters Reduced Functionality Mode (RFM).

## What RFM is

A sensor in RFM is installed and checking in, but isn't fully protecting the host. Usually caused by outdated sensor version, unsupported kernel, or broken install. It's a visibility gap, the host looks online in dashboards but isn't actually being protected.

## Flow

1. Trigger: Host state event - Host in Reduced Functionality Mode (RFM)
2. Get Device Details (enriches with hostname, OS, sensor version, last seen)
3. Slack v2 chat.postMessage with Block Kit formatting

## Slack message contents

Hostname, OS platform, sensor version, last seen timestamp, link to host management in Falcon.

## Sanitisation

- `SLACK_CONFIG_ID_HERE` - Slack integration config ID
- `SLACK_CHANNEL_ID_HERE` - target channel
- `ACTION_ID_PLACEHOLDER` - internal action instance IDs

## Variable references

All host details pulled from Get Device Details:

- `${data['GetDeviceDetails.Hostname']}`
- `${data['GetDeviceDetails.PlatformName']}`
- `${data['GetDeviceDetails.AgentVersion']}`
- `${data['GetDeviceDetails.LastSeen']}`

## Rebuild

1. Slack v2 integration set up in Falcon (see slack-setup.md)
2. New workflow, trigger: CrowdStrike Host state > Support > Host in Reduced Functionality Mode (RFM)
3. Add Get Device Details action, device_id wired from the trigger's device ID
4. Add Slack v2 chat.postMessage with Block Kit JSON from the YAML
5. Replace placeholders with real values
6. Save and enable (no easy way to test without forcing a real host into RFM; trust the wiring or wait for natural fire)

## Things worth knowing

- Hosts can enter and leave RFM legitimately during kernel updates or sensor upgrades, so some noise is expected.
- If noise becomes a problem, add a condition to only alert if the host has been in RFM for more than X minutes.
- A companion workflow for "host left RFM" could close the loop for audit purposes but isn't built.

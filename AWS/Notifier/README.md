# CloudHiro Notifier

## Description

CloudHiro Notifier sends notifications to Google Space, Slack, or Microsoft Teams based on user-defined parameters. This helps you stay informed about your AWS spending in real-time, allowing you to take proactive measures to manage your costs effectively.


## Deployment

To deploy CloudHiro Notifier, follow these steps:

1. Open CloudFormation Console using this link:
   - **[Deploy NotifyLambda Stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create?stackName=CloudHiroNotifier&templateURL=https://cloudhiro-public.s3.us-east-2.amazonaws.com/CloudHiroNotifier.yaml)**

2. Configure parameters:
   - `OverrideUrl`: Sends all notifications to this URL only (if set).
   - `CloudHiroNotifySpace`: URL for Google Space notifications.
   - `CloudHiroNotifySlack`: URL for Slack notifications.
   - `CloudHiroNotifyMeet`: URL for Microsoft Teams notifications.

3. Click **Create Stack** and wait for completion.


## Example Parameters

- Google Space URL: `https://chat.googleapis.com/v1/spaces/...`
- Slack URL: `https://hooks.slack.com/services/...`
- Microsoft Teams URL: `https://outlook.office.com/webhook/...`


## How It Works

1. The function reads the configured notification URLs.
2. If `OverrideUrl` is set, it sends all notifications to this endpoint.
3. Otherwise, it checks the other URLs and sends notifications accordingly.


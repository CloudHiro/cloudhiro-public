# CloudHiro Keep RDS Down

## Description

CloudHiro Keep RDS Down automation ensures that your RDS instances remain in a stopped state based on user-defined tags. This helps you manage your AWS costs effectively by preventing unnecessary running of RDS instances.


## Deployment

To deploy CloudHiro Keep RDS Down, follow these steps:

1. Open CloudFormation Console using this link:
   - **[Deploy KeepRDSDown Stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create?stackName=CloudHiroKeepRDSDown&templateURL=https://cloudhiro-public.s3.us-east-2.amazonaws.com/CloudHiroKeepRDSDown.yaml)**

2. Configure parameters:
   - `CloudHiroNotifierArn`: The ARN of the CloudHiro Notifier Lambda function to send notifications. (To find this go to [Stacks](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/), click on CloudHiroNotifier, go to the Outputs tab and copy the value)

3. Click **Create Stack** and wait for completion.

4. Tag any RDS instances you wish to keep stopped with `cloudhiro_rds_keep_down` tag.


## Example Tag Values

The `cloudhiro_rds_keep_down` tag value is the duration you wish to keep the RDS instance up before stopping it automatically and should be formatted as `max_up_<duration>`, where `<duration>` is a number followd by `m` or `h`.
- `max_up_30m`
- `max_up_2h`
- `max_up_1h`


## How It Works

1. CloudHiro Keep RDS Down uses EventBridge to detect when an RDS instance exceeds the allowed stop time and is being started.
2. If an instance is started, it checks for the `cloudhiro_rds_keep_down` tag.
3. If tag exists, the RDS instance is stopped after a wait time defined in tag value.
4. [CloudHiro Notifier](/AWS/Notifier/README.md) sends a notification.



# CloudHiro-Public

This repository contains AWS CloudFormation templates for managing and automating instances and sending notifications using AWS Lambda. 

Includes:
1. **NotifyLambda Stack** - A Lambda function that sends notifications to predefined endpoints.
2. **KeepRDSDown StateMachine** - A Step Functions state machine that stops RDS instances after being restarted automatically due to exceeding max stop time and triggers notifications.

---

## **1. NotifyLambda Stack**

### **Overview**
The **NotifyLambda** function is responsible for sending notifications to specified URLs (Google Space, Slack, or Microsoft Teams). It is deployed as an AWS Lambda function via CloudFormation.

### **Deployment**

1. Open CloudFormation Console using this link:
   - **[Deploy NotifyLambda Stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create?stackName=NotifyLambdaStack&templateURL=https://cloudhiro-public.s3.us-east-2.amazonaws.com/NotifyLambdaStack.yaml)**
2. Configure parameters:
   - `OverrideUrl`: Sends all notifications to this URL only (if set).
   - `CloudHiroNotifySpace`: URL for Google Space notifications.
   - `CloudHiroNotifySlack`: URL for Slack notifications.
   - `CloudHiroNotifyMeet`: URL for Microsoft Teams notifications.
3. Click **Create Stack** and wait for completion.

### **Updating & Deleting the Stack**
- Update: Go to CloudFormation, select **NotifyLambdaStack**, click **Update**.
- Delete: Select **NotifyLambdaStack**, click **Delete**.

---

## **2. KeepRDSDown StateMachine**

### **Overview**
The **KeepRDSDown StateMachine** automates the process of stopping an RDS instance when it exceeds a defined stop time. It integrates with the **NotifyLambda** function to send alerts.

### **Deployment**
1. Go to NotifyLambdaStack Outputs after it's deployed and copy the the ARN of the Notify Lambda function.
2. Open CloudFormation Console using this link:
   - **[Deploy KeepRDSDown StateMachine](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create?stackName=KeepRDSDown&templateURL=https://cloudhiro-public.s3.us-east-2.amazonaws.com/KeepRDSDownStateMachine.yaml)**
3. Configure parameters:
   - `NotifyLambdaArn`: The ARN of the Notify Lambda function just copied.
4. Click **Create Stack** and wait for completion.

### **Workflow**
1. **EventBridge Rule** detects when an RDS instance exceeds the allowed stop time.
2. **Step Functions State Machine** triggers `StopRDSInstance`.
3. **NotifyLambda** is invoked to send a notification.

### **Updating & Deleting the Stack**
- Update: Modify `KeepRDSDownStateMachine.yaml` and redeploy.
- Delete: Remove the CloudFormation stack associated with the state machine.

---

## **Conclusion**
This repository provides automation for managing AWS instances and sending notifications. The **NotifyLambda Stack** ensures notifications reach relevant users, while the **KeepRDSDown StateMachine** prevents unauthorized RDS instance uptime. Use the provided deployment links and troubleshooting guides to ensure smooth operation.


# Azure Service Bus Auto Downgrade

## Description

Cloudhiro Service Bus Auto Downgrade is a Logic App designed to help you optimize costs by automating the downgrade of Premium Azure Service Bus namespaces to Standard if they meet certain criteria. This helps you manage your Azure costs effectively by preventing unnecessary running of premium tier Service Bus instances.


## Deployment

To deploy Azure Service Bus Auto Downgrade, follow these steps:

1. Open Azure Portal and deploy the ARM template using this link:
    - **[Deploy ServiceBusAutoDowngrade Template](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCloudHiro%2Fcloudhiro-public%2Frefs%2Fheads%2FCLOUD-1659%2FAZR%2FServiceBusAutoDowngrade%2FCloudHiroServiceBusAutoDowngrade.json)**

    - **[Deploy ServiceBusAutoDowngrade Template (Main)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCloudHiro%2Fcloudhiro-public%2Frefs%2Fheads%2Fmain%2FAZR%2FServiceBusAutoDowngrade%2FCloudHiroServiceBusAutoDowngrade.json)**

2. Select project details (subscription, resource group) and instance details (region) for deployment.

3. Click **Review + Create** and wait for completion.

4. Tag any Service Bus instances you wish to manage with `cloudhiro-downgrade-servicebus`:`true` tag.


## How It Works

1. Cloudhiro Service Bus Auto Downgrade runs on a daily schedule.
2. Retrieves all Service Bus namespaces in the current subscription and resource group.
3. Filters for namespaces that:
    - Are Premium tier
    - Have a capacity (messaging units) of 1 or less
    - Are tagged with `cloudhiro-downgrade-servicebus`=`true`
4. If a namespace qualifies, creates a new Standard-tier Service Bus namespace. Name is based on the original name with a `-standard` suffix.
5. Migrates all relevant configurations and resources from the Premium namespace to the new Standard one:
    - Queues
    - Topics
    - Subscriptions - Retrieved per topic and recreated under the corresponding new topic.
6. After all resources are successfully cloned into the Standard namespace, the original Premium namespace is deleted to complete the downgrade.
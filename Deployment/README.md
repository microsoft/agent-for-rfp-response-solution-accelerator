# Deployment Guide

At a very high level, deploying this solution involves below key setups:

1. SharePoint Setup
2. Copilot Agent Setup
3. Power Automate Flow Setup
4. Microsoft Teams Setup

## Prerequisites

* [Power Platform environment](https://learn.microsoft.com/en-us/power-platform/admin/create-environment) with System Administrator access.
* Copilot Studio license.
* Permission to run and edit Power Automate flows.
* Access to create a SharePoint site.
* Access to an already existing Microsoft Teams channel or permissions to create a new Teams channel. Sample name: **Deal Room**.
* Access to Outlook.

If you run into issues, please refer to [Troubleshooting Guide](./TROUBLESHOOTING_GUIDE.md). 

## Step 1: SharePoint Site Setup

This will be a manual step as the Power Platform solution does not include a SharePoint site. This site is needed to store the incoming RFPs as well as the generated proposal documents.

### Step 1.1: Create SharePoint site

1. Go to (https://(your tenant).sharepoint.com/) and click **'Create site'.**
2. Select **Team Site.**
3. Select **Standard Team** template.
4. Enter name as desired.
5. **Save the URL** of this SharePoint site which will be used later to update environment variables. This will be in the format of: https://(your tenant).sharepoint.com/sites/(site name).

### Step 1.2: Create SharePoint Document Libraries

1. Create a new [SharePoint document library](https://support.microsoft.com/en-us/office/create-a-document-library-in-sharepoint-306728fe-0325-4b28-b60d-f902e1d75939#ID0EBF=Modern) and name it as desired, for example, "Original RFPs". This library will be used to store the incoming RFPs from customers.
2. Create a new [SharePoint document library](https://support.microsoft.com/en-us/office/create-a-document-library-in-sharepoint-306728fe-0325-4b28-b60d-f902e1d75939#ID0EBF=Modern) and name it as desired, for example, "RFP Proposals". This library will be used to store the agent's generated proposal for review.
3. Upload the RFP Template from the data folder to a document library (ex. Default document library) in same SharePoint site. This word template will be used to create the new RFP proposals.

## Step 2: Copilot Agent Setup

### Step 2.1: Import the Zip solution

1. The zip file in the [Solution](./Solution) folder contains all the components needed for provisioning the AI agent.
2. Import the zip file from the solution folder in this repository in the [PowerApps Maker Portal](https://make.powerapps.com) in the environment [provisioned earlier](#prerequisites) in prerequisites.
3. Click Next.
4. Click '**sign in'** for any connections that prompt to do so.
5. If you see any warnings with a yellow banner. Please ignore as we will update the parameters in subsequent steps.
6. After importing has completed, click **Publish all customizations** in the top menu. Wait for publishing to complete.
7. When the import is complete, the solution will be available in the environment. 

### Step 2.2: Import sample RFP documents

1. Go to Copilot Studio, click the knowledge tab.
2. Most of the times the sample RFP's will automatically start indexing which you will be able to see in the **Status** column.
3. However, if that's **not** the case, then please delete all the 7 documents and follow the below steps. If the documents are indexing and are 'Ready' then please skip steps 2.2.4-2.2.10.
4. From the [Data](./Data) folder of this repository, download all 7 documents with the same name you deleted from the previous step. Please note the [Data](./Data) folder contains 9 documents. Please exclude these two documents: `template.docx` and `Fourth Coffee-Explore Azure- AI-Services.docx`. 
5. Click on Add knowledge from the top left > In the Upload Files section > click to browse> select the 7 documents> Add.
6. Indexing these files will take a while, approximately 10-15 minutes.
7. Go to Topics > Click 'All' > Conversational Boosting > Create Generative Answers node.
8. Click **Edit** on data Sources.
9. Check all the documents uploaded.
10. Click **Save**.

Please refer to [Information on Sample Data](./INFORMATION_ON_SAMPLE_DATA.md) for details on sample data supplied for testing purpose. 

### Step 2.3: Enable Deep Reasoning

1. Navigate to Settings > Generative AI
2. Check the option **Use deep reasoning models**
3. Click **Save**
4. Note: This option should already be enabled as a part of the agent import but it's good to validate the same.

### Step 2.4: Set up Microsoft Teams channel (+M365 Copilot if applicable)

1. Click the **Channels** tab
2. In the Channels section, select **Teams + Microsoft 365**
3. If you have M365 Copilot licenses, we highly recommend publishing this agent in M365 copilot as well because it will allow you to use this agent in Microsoft Word Copilot for Users to update RFP's directly in word via this agent. Check the **Make agent available in M365 Copilot Chat**
4. Click **'Add channel'.**
5. Click **Save**.
6. This will set up the publishing channel(s) for Copilot Agent. 

### Step 2.5 Publish agent to Microsoft Teams channel (+M365 Copilot if applicable)

Publish your Copilot Agent by clicking '**Publish**'. If there are warnings, that is expected per the use case. This will make the copilot agent ready to be used in Teams.

This will publish the agent in Microsoft Teams and M365 Copilot if applicable.

## Step 3: Update Power Automate flows

There are three Power Automate flows in this solution. Some of those will need to be updated as covered below. To see a list of all flows, navigate to **'Flows**' section in the [PowerApps Maker Portal](https://make.powerapps.com).

#### Step 3.1: Update Flow - When a new RFP is received

1. Open the **When a new RFP is received** flow in **Edit mode** and click on the **'When a new email arrives'** action.
2. Select **Show advanced options**
3. In the **To** field, enter the email address on which to monitor for RFPs. Only when an email is sent to this email address, the agent will get triggered along with the filters covered below.
4. Optionally adjust the subject filter to specify the text to search for to trigger this flow. By default, the subject should contain "RFP".
5. Optionally adjust the Only with Attachments to No if it's not mandatory for a customer to send an attachment in the email when sending an RFP. By default, this is set to Yes.
6. **Save** the flow.

#### Step 3.2: Update Flow - Post draft RFP response

1. Open the **Post draft RFP response** flow in **Edit mode** and click on the **'Populate a Microsoft Word template'** action.
2. Update the SharePoint Site from Step 1.1, Library to the out of the box document library from Step 1.2.3, and file to the RFP Template from Step 1.2.3.
3. The Proposal field should auto populate from the copilot input of the same name from the first step.
4. Click on the **Create file** action and update the Site Address to the SharePoint site created in Step 1.1 and set the folder path to the "RFP Proposals" library created in Step 1.2
5. Repeat Step 4 on the **Get file properties** action
6. Click on the **Get files (properties only)** action and update the Site Address to the SharePoint site created in Step 1.1 and select the "Original RFPs" library created in Step 1.2.
7. In the **Post card in a chat or channel action**, select the Team you would like this Agent to send the draft proposals too and select the channel [provisioned earlier](#prerequisites) from the 5th point in the Prerequisites.
8. **Save** the flow.

#### Step 3.3: Update Flow - Copy attachment from email to SharePoint

1. Open the **Copy attachment from email to SharePoint flow** in **Edit mode** and click on the **'When a new email arrives'** action.
2. Select **Show advanced options**
3. In the **To** field, enter the same email address from **Step 2.4.1.3**
4. Click on the **Create file** action under the **Apply to each 2 action** and update the Site Address to the SharePoint site created in Step 1.1 and set the folder path to the "Original RFPs" library created in Step 1.2.
5. **Save** the flow.

#### Step 3.4: Ensure flows are turned on

Ensure all included flows are turned on. When viewing the list of flows included with the solution, the **Status** column will indicate whether each is On or Off. By selecting any flow, you will be able to turn it on by selecting **Turn On** in the top menu.

#### Step 3.5: Managing authentication for actions and flows (optional)

You will want to ensure that you configure any Copilot Studio actions, and any Connections used in Power Automate flows, to use the authentication that is appropriate for your organization and scenario.

Depending on your desired security model, you may wish to select **User authentication** for actions rather than **Agent author authentication**. For Power Automate flows, you may wish to configure connections to use the credentials of run-only users, or with a specific connection that you create.

See these resources for more details on setting up authentication:

* [Configure user authentication for actions](https://learn.microsoft.com/en-us/microsoft-copilot-studio/configure-enduser-authentication)
* [Manage owners and users in your flows with Power Automate](https://learn.microsoft.com/en-us/sharepoint/dev/business-apps/power-automate/guidance/manage-list-flows)

## Next Steps

After completing the deployment steps above, your Agent is now ready to use! Please visit our [Demo Script](./DEMO_SCRIPT.md) in this repo for demonstration and usage recommendations.

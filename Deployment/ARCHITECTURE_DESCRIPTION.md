# Architecture Description 

The architecture illustrates the integration and workflow that enables the RFP Response Agent accelerator to autonomously generate proposal responses from RFP submissions using Microsoft Power Platform, Copilot Studio, and Microsoft 365 services.

## **Overview**

The RFP Response agent is designed to streamline and automate the sales proposal process in response to incoming RFPs. By leveraging Copilot Studio, Power Automate, Dataverse, and SharePoint, the agent can autonomously trigger, use deep reasoning, retrieve knowledge, and orchestrate actions to support a sales manager through Teams or Outlook. This ensures timely and context-rich responses to customer RFPs, while also maintaining compliance and leveraging centralized data sources.

## **Components and Roles**

**Copilot Studio** is the core product used to configure, publish, and manage the agent. Within it, RFP & product specific general instructions, triggers, actions and knowledge sources are maintained. The agent is published to Microsoft Teams, allowing direct and natural interaction with sellers inside their daily flow of work. GenAI capabilities within Copilot Studio enable reasoning over questions not explicitly covered by a preconfigured topic.

**RFP Response Agent** is the deployed virtual agent that listens to signals from Outlook, Teams, or Power Automate. It includes:

- **Autonomous Triggers** to act based on events such as new RFPs received.
- **Deep Reasoning** to create custom project plans, compliance practices and confidence score.
- **Knowledge Source Q&A** to pull relevant content from connected files and SharePoint libraries.
- **GenAI Orchestration** to intelligently construct proposal responses.
- **Instructions and Actions** to guide execution and next steps in the proposal process.
- **Power Automate** acts as the workflow engine for both reactive and proactive flows. It helps connect the agent with backend data systems like Dataverse and SharePoint, enabling it to retrieve raw RFP inputs, update proposal statuses, and store generated content.
- **Dataverse** serves as the structured data store for RFP documents. 
- **SharePoint** stores both raw RFP documents from customers and the agent-generated proposal responses. It serves as a document library and knowledge repository that the agent can pull from during generation and store into upon completion.
- **Microsoft Teams** provides the engagement layer, allowing the Sales Manager to chat with the agent in real time, either reactively after a trigger or proactively for support in generating or customizing proposals.

## **Key Flow Summary**

1. A **Customer** sends an RFP, which is detected via **Outlook** and **Power Automate**.
2. The agent processes the request using **GenAI orchestration**, **Deep reasoning**, tapping into **Q&A** from knowledge sources, and performs **actions** as required.
3. Agent posts the **generated proposal in a channel in Teams** where all the sellers can effectively collaborate on the proposal & ask questions as desired.
4. Final proposals are stored back in **SharePoint**, while data compliance and project tracking info are logged in **Dataverse**.

**Disclaimer** 

Terms displayed or discussed are for explanation/education only. Such terms may differ from your agreement.
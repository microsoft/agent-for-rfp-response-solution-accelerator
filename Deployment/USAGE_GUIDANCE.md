# Usage Guidance

Once this solution has been deployed by following all the steps from the [Deployment Guide](Deployment/README.md), here are some important pointers on how the various components this solution function.

## **Functionality Overview**

Here is a high level overview of each important components of the solution:

## **Email Trigger**

 There is no specific format for receiving email from the customer who is submitting RFP (Request for Proposal) however for the solution to work the email should have a *subject with word ‘RFP’* and the *email body should have a reference to the product/service* to match against the the document(s) in the knowledge base of the agent with the same name. For example, if the email sent has 'Azure Landing Zones' in the body of the email and there is a knowledge source document of the same name, the agent will be able to match the names and create a draft proposal.

**Note**: If needed, this solution can be extended by integrating with CRM systems like Dynamics 365 which would allow to identify customer names, match on products already existing in the opportunity records in the database and such.

## **RFP Proposal Template**

The sample word template being leveraged here can be updated as desired. The important function to note in this word template is the usage of the '**Proposal'** field which is where the agent inputs all of the content it generates for the proposal. If you'd like to have more fields, replace logos or use multiple templates, please feel free to update the template as needed.

## Power Automate workflows

There are 3 power automate workflows that are leveraged in this solution.

#### Post Draft RFP Response

This flow is leveraged to create the draft proposal document using the word template mentioned above, save a copy of that in SharePoint and finally post the created document in Microsoft Teams using an adaptive card. These 3 outcomes are achieved through actions in this flow. Each of these actions are covered in the deployment readme where these can be updated as needed.

#### Copy attachment from email to SharePoint

This flow is used to copy the RFP attachment sent in the email from the customer and store the document in a library in SharePoint. The setup of this is covered in the deployment readme. The purpose of this flow is to allow the users to be able to see reference the original RFP document as desired to update the AI generated RFP document.

#### When a new RFP is recieved

This flow is the autonomous trigger for the agent to start the proposal creation as per it's instructions. The most important function to note here is that this flow filters the emails to only process the ones whose subject contains "RFP" and an attachment along with the mailbox which will be intended to recieve the RFP's. If you wish to change that, please update the flow as per the instructions in the deployment readme.

## RFP Response Agent

The RFP response agent performs the following key functions when the autonomous trigger is activated when the email is recieved to the intended mailbox-

1. Matches the product name from the email to the document name from it's knowledge base.
2. Upon match, it leverages the matched document from the knowledge base to start drafting a detailed proposal.
3. Using deep reasoning, the agent takes today's date as a variable and creates a detailed project plan for the delivery of the RFP.
4. Using deep reasoning, the agent considers the compliance & security best practices from the knowledge base and includes it in the proposal based on the product the RFP is being drafted for.
5. Using deep reasoning, the agent comes up with a confidence score on how confident it feels on the proposal being sent over.
6. Finally, the agent takes the output of the above points in a variable and passes it as an input to the **'Post Draft RFP Response' power automate workflow. If you wish to use another flow or not use this flow, please update the instructions of the agent.**

## **Teams Channel**

 The notification in the teams channel is governed by the adaptive card designed in the Post Draft RFP Response power automate flow. The intention of this card is to intimate channel team members about draft RFP response document available for review and other link to open the RFP document submitted by customer. Please @ the agent when the user intends to chat with the agent. This applies in Teams and also Word Copilot if you have M365 copilot licenses.

## SharePoint

SharePoint is leveraged to store the Generated RFP's and Raw RFP's in 2 libraries as covered in the deployment readme. Please update these locations as desired in the power automate workflows covered above.

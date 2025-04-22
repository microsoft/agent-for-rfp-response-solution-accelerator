# Differentiators

There are several capabilities that were focused on as key differentiators for this accelerator. Their usage and business value is discussed here.

## Deep reasoning in Copilot Studio

Deep reasoning uses the Azure OpenAI o1 model to complete tasks requiring logical reasoning, problem solving, and step-by-step analysis. This capability is leveraged in this solution for creating the proposal based on leveraging existing knowledge sources vs the content from the customer RFP, deep reasoning is also used to create customized project plans, compliance & security best practices and coming up with a confidence score.

This feature enhances user productivity manifold as it saves them all the time they would spend looking for information manually in past RFP's, go over the compliance policies and come up with a project plan. This capability does all of the above work automatically enhancing productivity.

Learn more about deep reasoning: [Deep Reasoning in Copilot Studio](https://www.youtube.com/watch?v=_v9ri9eoVFg)

## Autonomous triggers in Copilot Studio

Autonomous triggers in copilot studio agents perform actions in response to something happening using *event triggers.* Event triggers allow your agent to act autonomously in response to the defined event occurring. In this solution, we are leveraging an autonomous trigger when an email with an attachment and a subject that contains "RFP" is received to act as the event which triggers the agent.

This capability differentitates the solution as it results in process automation without requiring any input from users. The agent is able to fully process the document, create the proposal and post it in Teams where the users are working without any human interaction. This results in increased productivity and automation of manual tasks.

Learn more about autonomous triggers: [Autonomous triggers in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-triggers-about)

## Knowledge sources in Copilot Studio

With Copilot Studio, knowledge sources act in concert with generative answers. When knowledge sources are added, agents can use enterprise data. Knowledge sources allow for agents to provide relevant information when desired. In this solution, all of the previously completed RFP's are used as knowledge sources which ground the agent in using them as a source of data to provide answers, generate proposals and give insights based on the RFP document recieved by the customer.

This is an important differentiator as it automates the entire process of a user not having to go over all the previously completed RFP's manually to search for information related to the recieved customer RFP. The agent being able to automatically retreive that data, put it in a proposal document and give insights to the user is a huge productivity booster.

Learn more about knowledge sources: [Knowledge Sources in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-studio)

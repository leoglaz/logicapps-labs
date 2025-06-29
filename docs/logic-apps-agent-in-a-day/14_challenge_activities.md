# Module 14 - Challenge Activities

## Challenge 1 - Add a new tool for email notification
Create a new tool that will be send an email the requestor providing them updates when the indicent is created and when the incident is resolved.

*Hints:*
- Leverage  existing Logic Apps Connectors such as *Outlook.com*, *Office 365 Outlook* or *Gmail* to simpfly interacting with the email clients
- Leverage agents parameters when populating the contents of your email
- Tools include one or actions

## Challenge 2 - Make the workflow autonomous 
In this challenge you will modify the workflow to be execute in an autonomous manner. 

An autonomous workflow will require the user to provide all parameters in advance and potentially return a response.

*Hints:*
- The trigger action `When a HTTP request is received` will need to accept all inputs that are required for the agent to process the event.
- The System Instructions will need be refined as there will be no user interaction between the agent and user requesting additional input variables or validation
- The `Complete Workflow` action can be used to stop the workflow once have completed closed the ServiceNow indicent and sent out the notification.

## Challenge 3 - Updating the Knowledgebase
When the agent encounters a new scenario not present in the knowledgebase the agent will:
- Update the knowledgebase with the new scenario
- Send a notification communication




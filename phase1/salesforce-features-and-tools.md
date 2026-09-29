# Phase 1: Identifying Key Salesforce Features & Tools Required

## 1. Purpose

This document identifies the Salesforce features, automation components, Agentforce capabilities, data relationships, and supporting tools required to implement the Customer Support Ticket Priority Prediction and Automated Assignment System Using Agentforce.

The selected Salesforce features are intended to support the complete workflow from customer ticket identification and description analysis through priority classification, assignment, urgent-task creation, and conversational output.

---

## 2. Technology Platform

### Salesforce

Salesforce is the main application platform for the project.

It is used to:

- Store support-ticket information.
- Maintain customer Account and Contact relationships.
- Store ticket priority and status.
- Maintain support-agent assignment information.
- Execute Flow-based automation.
- Store Tasks generated for high-priority tickets.
- Provide the environment in which Agentforce interacts with the backend automation.

---

## 3. Custom Object

### Support Ticket Intelligence

A custom Salesforce object named:

```text
Support Ticket Intelligence

4. Custom Fields and Relationships
Account Relationship

The Customer__c field is a Lookup relationship to the Salesforce Account object.

This allows a support ticket to be associated with a specific customer Account.

The Account relationship is important because the project's Flow accepts an Account Name and then retrieves the latest support ticket associated with that Account.

Contact Relationship

The Contact__c field connects the support ticket to a customer Contact.

This provides additional customer information associated with the support request.

User Relationship

The Assigned_To__c field is a Lookup to the Salesforce User object.

It represents the support agent assigned to handle the ticket.

Ticket Information Fields

The remaining fields capture the information required for analysis and support handling:

Issue Type
Description
Priority Level
Status
Created Date
SLA Breach Risk
Resolution Time
5. Auto-Launched Flow
Purpose

An Auto-Launched Flow is used as the backend automation mechanism for ticket analysis and handling.

Unlike a record-triggered flow, this Flow is designed to be called without a record-trigger and can be invoked from Agentforce.

Flow Type
Auto-Launched Flow
(No Trigger / Called from Agentforce)
Main Responsibilities

The Flow is designed to:

Receive the Account Name as input.
Find the corresponding Account.
Store the Account ID.
Retrieve the latest support ticket for that Account.
Store the Ticket ID.
Analyze the ticket description.
Determine ticket priority.
Check whether the ticket is High priority.
Create an urgent handling Task for High-priority tickets.
Assign the ticket to the appropriate support level.
Optionally check SLA risk.
Generate a final action message.
Return the results to Agentforce.
6. Flow Input Variable
Account Name

The Flow uses the following input variable:

Label	API Name	Type	Input
Account Name	varAccountName	Text	Yes

The Account Name is supplied to the Flow so that it can identify the corresponding Salesforce Account.

7. Flow Output Variables

The Flow returns the following outputs:

Label	API Name	Type	Purpose
Account Id	varAccountId	Text	Returns the Account identifier
Ticket Id	varTicketId	Text	Returns the analyzed ticket identifier
Priority Level	varPriorityLevel	Text	Returns High, Medium, or Low
Assigned Agent	varAssignedTo	Text	Returns the assigned support agent
Action Message	varActionMessage	Text	Returns the final action/status message

These outputs are used by Agentforce to provide the result to the user.

8. Get Records – Account Retrieval

The first major Flow operation retrieves the customer Account.

Element
Get Account
Object
Account
Condition
Name Equals {!varAccountName}
Sorting
CreatedDate DESC
Record Storage

Only the first matching record is stored.

Purpose

This step converts the Account Name supplied to the Flow into the corresponding Salesforce Account record and Account ID.

9. Assignment – Store Account ID

After retrieving the Account, an Assignment element stores its ID.

Element
Store Account Id
Assignment
varAccountId = {!Get_Account.Id}

The Account ID is then used to locate the customer's latest support ticket.

10. Get Records – Latest Ticket Retrieval

The next Flow step retrieves the customer's latest support ticket.

Element
Get Ticket
Object
Support_Ticket_Intelligence__c
Condition
Customer__c Equals {!varAccountId}
Sorting
CreatedDate DESC
Record Storage

Only the first matching record is stored.

Purpose

This ensures that the Flow works with the latest support ticket associated with the specified Account.

11. Assignment – Store Ticket ID

The Flow stores the ID of the retrieved ticket.

Element
Store Ticket Id
Assignment
varTicketId = {!Get_Ticket.Id}

The Ticket ID is later used when creating a Task and returning the ticket information to Agentforce.

12. Decision Logic – Analyze Description

The Flow uses a Decision element to analyze the ticket description.

Element
Analyze Description

The documented implementation uses configured keyword conditions.

High Priority Conditions

The description is treated as High priority when it contains:

urgent
not working
failure
Medium Priority Conditions

The description is treated as Medium priority when it contains:

issue
slow
delay
Default Outcome

If none of the configured High- or Medium-priority conditions are matched:

Low Priority
Priority Decision
Ticket Description
        |
        +---- contains urgent / not working / failure
        |                |
        |                v
        |              HIGH
        |
        +---- contains issue / slow / delay
        |                |
        |                v
        |             MEDIUM
        |
        +---- none of the above
                         |
                         v
                        LOW
13. Assignment – Set Priority Level

After the Decision element determines the priority path, an Assignment element stores the result.

High Path
varPriorityLevel = "High"
Medium Path
varPriorityLevel = "Medium"
Low Path
varPriorityLevel = "Low"

This variable is used by later Flow logic and returned to Agentforce.

14. Decision Logic – High Priority Check

A second Decision element checks whether the calculated priority is High.

Element
Is High Priority
Condition
varPriorityLevel Equals "High"

This determines whether the urgent-ticket automation should execute.

15. Task Automation

High-priority tickets require an urgent handling Task.

Element
Create Task
Object
Task
Task Fields
Field	Value
Subject	Urgent Ticket Handling
WhatId	{!varTicketId}
Priority	High
Status	Not Started
Purpose

This automation ensures that High-priority tickets generate an explicit handling task.

16. Assignment Logic

The project uses Flow Assignment logic to determine the support level.

For High-priority tickets, the documented implementation assigns the ticket to:

Senior Support Agent

The Flow stores this value in:

varAssignedTo
Assignment
varAssignedTo = "Senior Support Agent"

This provides the assigned support level as part of the Flow output.

17. SLA Risk Checking

The project includes an optional SLA-risk decision.

Example Condition
Created Date older than 2 days

When the condition is satisfied, the Flow can set or return an internal SLA-risk indication or message.

Purpose

This provides awareness of potentially delayed unresolved tickets.

The current project scope treats the SLA check as an optional/basic risk check rather than a complete SLA monitoring and escalation system.

18. Final Output Message

The Flow generates a final message according to the calculated priority.

High Priority
High priority ticket detected. Assigned to senior agent.
Medium Priority
Ticket marked as medium priority. Will be handled shortly.
Low Priority
Tickets are low priority and queued for processing.

The message is stored in:

varActionMessage

and returned to Agentforce.

19. Agentforce
Purpose

Agentforce provides the AI-assisted conversational interface for the ticket-priority system.

The project uses an Agentforce subagent named:

Support Ticket Priority Analysis
API Name
Support_Ticket_Priority_Analysis
Responsibilities

The subagent is intended to:

Analyze support-ticket descriptions.
Determine ticket priority.
Retrieve the latest support ticket for an Account.
Trigger the backend Auto-Launched Flow.
Initiate urgent-task creation for High-priority tickets.
Provide the resulting priority and assignment information to the user.
20. Agentforce Topic / Subagent Scope
Name
Support Ticket Priority Analysis
Classification Description

The topic analyzes customer support tickets to determine priority based on issue description and urgency. It handles requests related to ticket status, issue severity, and support prioritization.

Scope

The subagent is focused on:

Support-ticket description analysis.
Priority determination.
Backend automation for task assignment.

The documented scope excludes unrelated requests such as billing, subscription management, or account updates.

21. Agentforce Instructions

The documented Agentforce interaction follows this general sequence:

1. Ask for Account Name if not already provided.
2. Retrieve the latest support ticket for the Account.
3. Read the ticket description.
4. Analyze the configured urgency conditions.
5. Determine High, Medium, or Low priority.
6. For High priority, trigger the Auto-Launched Flow.
7. Ensure urgent handling Task creation.
8. Assign the appropriate support level.
9. Return a clear response to the user.
10. Use Flow for backend actions rather than directly updating records or sending emails.
22. Agentforce Input

The primary input is:

Input	API Name	Data Type	Required
Account Name	varAccountName	lightning__textType	Yes
Description

The user provides the customer Account Name so that the system can locate the customer's latest support ticket and analyze its priority.

23. Agentforce Outputs

The configured Agentforce action returns:

Account ID
varAccountId

Returns the unique identifier of the customer Account used to retrieve the ticket.

Ticket ID
varTicketId

Returns the unique identifier of the analyzed support ticket.

Priority Level
varPriorityLevel

Returns the calculated priority:

High
Medium
Low
Assigned Agent
varAssignedTo

Indicates the support agent or support level assigned to the ticket.

Action Message
varActionMessage

Displays the final message describing the ticket priority and action taken.

These outputs can be displayed in the Agentforce conversation.

24. Flow and Agentforce Integration

The planned integration can be represented as:

User
  |
  v
Provides Account Name
  |
  v
Agentforce
  |
  v
Support Ticket Priority Analysis
  |
  v
Auto-Launched Flow
  |
  +--> Get Account
  |
  +--> Get Latest Ticket
  |
  +--> Analyze Description
  |
  +--> Determine Priority
  |
  +--> High Priority Check
  |       |
  |       +--> Create Urgent Task
  |
  +--> Assign Support Level
  |
  +--> Optional SLA Risk Check
  |
  +--> Generate Final Message
  |
  v
Agentforce Response

The project's documented architecture follows this Agentforce → subagent → Auto-Launched Flow → ticket analysis → priority/assignment → response pattern.

25. Reports and Dashboards

Salesforce Reports and Dashboards will provide basic visibility into support operations.

Potential information to monitor includes:

Ticket status
Ticket priority
Issue type
Assigned support agent
High-priority tickets
SLA breach risk
Resolution information

The project scope describes visibility for support agents and managers, while more advanced analytics are treated as future scope.

26. Security and Access

Basic access control will be used to ensure that customer and ticket information is available only to authorized users.

The planned user groups are:

Support Agents

Access required to handle assigned support tickets and view relevant ticket information.

Support Managers

Access required to monitor support tickets, priorities, workload, and support handling.

System Administrator

Access required to configure Salesforce objects, fields, Flow automation, Agentforce, and related configuration.

The requirement is to maintain controlled Salesforce access to customer and ticket information.

27. Feature-to-Requirement Mapping
Salesforce Feature	Requirement Supported
Custom Object	Support ticket creation and management
Account Lookup	Customer Account association
Contact Lookup	Customer Contact association
Description field	Ticket description analysis
Priority Level field	Automatic priority classification
Status field	Ticket-status management
Assigned To field	Support agent assignment
Auto-Launched Flow	Backend ticket automation
Flow Variables	Input and output handling
Flow Decision	Priority classification
Flow Assignment	Priority and assignment values
Create Records	High-priority Task creation
SLA Decision	SLA breach-risk checking
Agentforce	Conversational ticket analysis
Agentforce Topic/Subagent	Ticket-priority analysis
Reports/Dashboards	Ticket visibility and monitoring
Salesforce Access Controls	Basic security
28. Overall Feature Architecture
                   SALESFORCE
                       |
        +--------------+--------------+
        |                             |
        v                             v
 Custom Object                   Agentforce
        |                             |
        |                    Support Ticket Priority
        |                         Analysis
        |                             |
        +-------------+---------------+
                      |
                      v
               Auto-Launched Flow
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
 Get Records      Decision       Assignment
       |              |              |
       v              v              v
Account/Ticket    Priority       Agent
       |              |
       |              v
       |        High Priority?
       |              |
       |              v
       |        Create Task
       |              |
       +--------------+-------------+
                      |
                      v
                Final Message
                      |
                      v
              Agentforce Response
29. Key Tools Required
Tool / Feature	Purpose
Salesforce	Main application platform
Object Manager	Custom object and field configuration
Support Ticket Intelligence	Main ticket data object
Flow Builder	Backend automation
Auto-Launched Flow	Ticket retrieval and processing
Flow Variables	Input/output data handling
Flow Decision	Priority and SLA decisions
Flow Assignment	Store priority and assignment values
Create Records	Create urgent Tasks
Agentforce	AI-assisted conversational analysis
Agentforce Topic / Subagent	Define ticket-priority behavior
Reports	Ticket and performance reporting
Dashboards	Basic management visibility
Profiles / Roles / Access Controls	Controlled access
30. Expected Tool Interaction

The complete planned system interaction is:

Salesforce
    |
    +---- Support Ticket Intelligence
    |         |
    |         +---- Account
    |         +---- Contact
    |         +---- Description
    |         +---- Priority
    |         +---- Status
    |         +---- Assigned To
    |         +---- SLA Risk
    |         +---- Resolution Time
    |
    +---- Flow Builder
    |         |
    |         +---- Retrieve records
    |         +---- Analyze description
    |         +---- Set priority
    |         +---- Create Task
    |         +---- Assign support level
    |         +---- Generate output
    |
    +---- Agentforce
              |
              +---- Receive Account Name
              +---- Analyze ticket
              +---- Trigger Flow
              +---- Display results
31. Conclusion

The project will use Salesforce as the main application and data platform, an Auto-Launched Flow for backend automation, and Agentforce for conversational ticket analysis.

The key Salesforce features include the Support Ticket Intelligence custom object, Account/Contact/User relationships, Flow variables and decision logic, automatic Task creation, support assignment, basic SLA-risk checking, Agentforce topic configuration, and output handling.

Together, these components provide the technical foundation required to implement the project's support-ticket prioritization and automated assignment workflow.

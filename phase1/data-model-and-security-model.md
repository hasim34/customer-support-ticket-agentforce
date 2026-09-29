# Phase 1: Designing Data Model and Security Model

## 1. Purpose

This document defines the proposed data model and security model for the Customer Support Ticket Priority Prediction and Automated Assignment System Using Agentforce.

The data model is designed to store customer support ticket information, maintain relationships with customer Accounts and Contacts, track ticket priority and status, record support-agent assignment, and support SLA-risk tracking.

The security model is designed to provide controlled access to customer and ticket information for support agents, managers, and administrators.

---

# 2. Data Model

## 2.1 Main Custom Object

The primary object for the project is:

**Object Label:**

`Support Ticket Intelligence`

**API Name:**

`Support_Ticket_Intelligence__c`

The custom object acts as the main data store for support-ticket information.

---

## 2.2 Ticket Fields

The project document defines the following fields.

| Field Label | API Name | Data Type | Description |
|---|---|---|---|
| Ticket Number | `Ticket_Number__c` | Auto Number | Unique ticket number using `TKT-{0000}` format |
| Customer | `Customer__c` | Lookup (Account) | Related customer Account |
| Contact | `Contact__c` | Lookup (Contact) | Customer contact |
| Issue Type | `Issue_Type__c` | Picklist | Technical, Billing, General |
| Description | `Description__c` | Long Text Area | Support-ticket issue details |
| Priority Level | `Priority_Level__c` | Picklist | Low, Medium, High |
| Status | `Status__c` | Picklist | New, In Progress, Resolved |
| Created Date | `Created_Date__c` | Date | Ticket creation/date information |
| Assigned To | `Assigned_To__c` | Lookup (User) | Support agent assigned to the ticket |
| SLA Breach Risk | `SLA_Breach_Risk__c` | Checkbox | Indicates potential SLA risk |
| Resolution Time (hrs) | `Resolution_Time__c` | Number | Time taken to resolve the ticket |

---

# 3. Relationships

## 3.1 Customer → Account

The `Customer__c` field is a Lookup relationship to the Salesforce **Account** object.

### Purpose

This relationship associates each support ticket with the customer Account.

The Account relationship is also used by the project's Flow. The Flow accepts an Account Name as input, retrieves the matching Account, and then uses the Account ID to retrieve the latest support ticket.

---

## 3.2 Contact → Contact

The `Contact__c` field is a Lookup relationship to the Salesforce **Contact** object.

### Purpose

This relationship identifies the customer contact associated with a support ticket.

It allows the support-ticket record to retain both Account-level and Contact-level customer information.

---

## 3.3 Assigned To → User

The `Assigned_To__c` field is a Lookup relationship to the Salesforce **User** object.

### Purpose

This field identifies the support agent assigned to handle the ticket.

The project uses assignment logic to determine the appropriate support level. In the documented implementation, a High-priority ticket is assigned to a **Senior Support Agent**.

---

# 4. Data Model Structure

The high-level relationship structure is:

```text
                 Salesforce Account
                        |
                        | 1
                        |
                        | *
                        v
            Support Ticket Intelligence
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
       Contact      Assigned To    Ticket Data
                     (User)        Fields

5. Ticket Data Categories

The fields can be grouped according to their purpose.

5.1 Identification
Ticket Number
Created Date

These identify and timestamp the support ticket.

5.2 Customer Information
Customer
Contact

These associate the ticket with the relevant customer Account and Contact.

5.3 Issue Information
Issue Type
Description

These describe the nature of the customer's problem.

5.4 Processing Information
Priority Level
Status
Assigned To

These fields track how the ticket is being handled.

5.5 SLA / Resolution Information
SLA Breach Risk
Resolution Time (hrs)

These fields support delayed-ticket and resolution tracking.

6. Picklist Values
6.1 Issue Type

The project defines:

Technical
Billing
General
6.2 Priority Level

The project defines:

Low
Medium
High
6.3 Status

The project defines:

New
In Progress
Resolved
7. Ticket Processing Data Flow

The data model is designed to support the following processing flow:

Account Name
      |
      v
Find Account
      |
      v
Store Account ID
      |
      v
Find Latest Support Ticket
      |
      v
Read Ticket Description
      |
      v
Analyze Description
      |
      v
Determine Priority
      |
      v
Determine Assignment
      |
      v
Create Urgent Task if High
      |
      v
Return Processing Result

The project specifically defines the sequence:

Account → Latest Support Ticket → Ticket Description → Priority Decision → Agent Assignment → High-Priority Task → Final Response.

8. Flow Data Usage

The Auto-Launched Flow interacts with the data model through input and output variables.

8.1 Input
Account Name
varAccountName

Type:

Text

Purpose:

Receives the customer Account Name used to locate the Account and its latest support ticket.

8.2 Outputs
Output	API Name	Purpose
Account Id	varAccountId	Returns the Account identifier
Ticket Id	varTicketId	Returns the analyzed ticket identifier
Priority Level	varPriorityLevel	Returns High, Medium, or Low
Assigned Agent	varAssignedTo	Returns the support agent/support level
Action Message	varActionMessage	Returns the final processing message
9. Data Model and Automation Relationship

The data model supports the Auto-Launched Flow as follows:

Support Ticket Intelligence
             |
             +---- Customer__c
             |       |
             |       v
             |     Account
             |
             +---- Contact__c
             |       |
             |       v
             |     Contact
             |
             +---- Description__c
             |
             +---- Priority_Level__c
             |
             +---- Status__c
             |
             +---- Assigned_To__c
             |       |
             |       v
             |      User
             |
             +---- SLA_Breach_Risk__c
             |
             +---- Resolution_Time__c
10. Security Model
10.1 Security Objective

The security model is intended to provide controlled Salesforce access to customer and support-ticket information.

The project identifies security as a non-functional requirement and specifies:

Controlled Salesforce access to customer and ticket information.

The detailed implementation of specific profiles, roles, sharing rules, and field-level security is treated here as the planned security model for the project.

11. User Roles

The main user categories are:

11.1 Support Agent

Support agents handle assigned support tickets.

Planned access

Support agents should be able to:

Create support tickets when permitted.
View relevant support tickets.
View ticket descriptions.
View priority and status.
Handle assigned tickets.
Update ticket status during support handling.
11.2 Support Manager

Support managers monitor support operations and team workload.

Planned access

Managers should be able to:

View support tickets across the support team.
Monitor priority.
Monitor ticket status.
View assignment information.
Monitor SLA-risk information.
Review support-ticket handling.
11.3 System Administrator

The administrator configures the Salesforce environment.

Planned access

The administrator should be able to:

Configure objects and fields.
Configure relationships.
Build and modify Flow automation.
Configure Agentforce.
Configure permissions and access settings.
Maintain reports and dashboards.
12. Profiles and Permissions

The planned permission model is:

User Type	Planned Access
Support Agent	Create and view relevant support tickets; handle assigned tickets
Support Manager	View and manage team tickets, priority, and assignment information
System Administrator	Full configuration and administration access

Access should follow the minimum permissions required for each user role.

13. Record-Level Visibility

The planned record-visibility model is:

Support Agent
      |
      v
Assigned / Relevant Tickets

Support Manager
      |
      v
Team Tickets

System Administrator
      |
      v
All Required Project Records

This provides role-based visibility according to the responsibilities of each user.

14. Sharing Rules

The planned sharing model should restrict unnecessary exposure of support-ticket records.

Support Agents

Agents should primarily have access to tickets relevant to their support responsibility or assigned workload.

Support Managers

Managers should have visibility across their team's support tickets so that they can monitor workload, priority, status, and assignment.

Administrator

The administrator requires broader access for system configuration and maintenance.

15. Field-Level Security

Important ticket-processing fields should have controlled edit access.

Examples include:

Priority Level
SLA Breach Risk
Assigned To

A possible permission approach is:

Field	Support Agent	Support Manager	Administrator
Priority Level	View / controlled update	View / Manage	Full control
SLA Breach Risk	View	View / Manage	Full control
Assigned To	View / as permitted	Manage	Full control
Description	View / update as permitted	View / Manage	Full control
Status	Update during handling	View / Manage	Full control

These permissions should be configured according to the team's Salesforce security requirements.

16. Data Visibility Through Role Structure

The planned role structure is:

                 Support Manager
                       |
          +------------+------------+
          |            |            |
          v            v            v
       Agent 1       Agent 2      Agent 3

The manager can have visibility into team tickets, while agents have access appropriate to their responsibilities.

17. Security Considerations

The system should protect:

Customer Account information.
Customer Contact information.
Ticket descriptions.
Ticket priority.
Assignment information.
SLA-risk information.
Resolution information.

Security configuration should prevent unauthorized users from viewing or modifying protected information.

18. Data Integrity Considerations

The data model should maintain accurate relationships and ticket information.

Important considerations include:

Each ticket should be associated with the correct customer Account.
Contact information should correspond to the relevant customer.
Ticket priority should follow the configured Flow logic.
Assignment should reflect the appropriate support level.
SLA-risk information should be maintained consistently.
Resolution time should reflect ticket handling duration.
19. Data Model to Security Mapping
Data / Function	Security Requirement
Customer Account	Controlled customer-data access
Contact	Controlled contact-data access
Ticket Description	Available only to authorized support users
Priority Level	Controlled editing
Assigned To	Controlled assignment access
SLA Breach Risk	Controlled viewing/editing
Resolution Time	Controlled ticket-management access
Support Ticket	Role-based record visibility
20. Overall Data and Security Architecture
                    Salesforce Org
                         |
          +--------------+--------------+
          |                             |
          v                             v
   Data Model                     Security Model
          |                             |
          v                             v
Support Ticket Object          User Roles / Profiles
          |                             |
   +------+------+              +-------+-------+
   |      |      |              |       |       |
Account Contact User          Agent  Manager  Admin
   |      |      |              |       |       |
   +------+------|              +-------+-------+
          |                               |
          v                               v
      Ticket Data                 Record / Field Access
          |
          v
     Flow Automation
          |
          v
      Agentforce
21. Design Summary

The data model is centered on the Support_Ticket_Intelligence__c custom object.

The object stores:

Ticket identification
Customer information
Contact information
Issue information
Priority
Status
Assignment
SLA risk
Resolution time

The relationships with Account, Contact, and User allow the system to associate each support ticket with a customer, customer contact, and assigned support user.

The security model is based on controlled access according to user responsibilities, with broader visibility for managers and administrators and more limited access for support agents.

22. Conclusion

The proposed data model provides the Salesforce structure required to support ticket retrieval, analysis, priority classification, assignment, SLA-risk tracking, and resolution tracking.

The security model provides a planned approach for protecting customer and support-ticket information through role-based access, permissions, record visibility, and controlled access to important fields.

Together, the data and security models provide the foundation for the Flow automation and Agentforce configuration that follow in later project phases.





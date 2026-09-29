# Phase 1: Requirement Analysis & Planning

## 1. Project Title

**Customer Support Ticket Priority Prediction and Automated Assignment System Using Agentforce**

## 2. Project Overview

Customer support teams receive a large number of tickets daily, while prioritization and assignment can be manual. This project provides a Salesforce-based solution that analyzes ticket information, classifies urgency, assigns the appropriate support level, and initiates automation for critical cases.

The solution uses Salesforce and Agentforce to analyze customer support ticket descriptions, automatically determine priority as High, Medium, or Low, and trigger backend automation for appropriate support handling.

## 3. Purpose

The system is designed to:

- Automatically classify support tickets as High, Medium, or Low priority.
- Retrieve the latest support ticket associated with a customer Account.
- Reduce manual prioritization and assignment effort.
- Create an urgent handling task for High-priority tickets.
- Use Agentforce to analyze ticket details and trigger backend automation.
- Improve ticket response and resolution efficiency.

## 4. Problem Statement

Support teams handle large numbers of tickets, and ticket prioritization may require manual decisions. Important urgency signals in ticket descriptions can be missed, resulting in delays in handling critical issues.

The main problems identified are:

- Manual ticket prioritization.
- Critical tickets not always being identified first.
- Manual support-agent assignment.
- Urgency signals being missed in ticket descriptions.
- Older unresolved tickets potentially creating SLA-related risk.

## 5. User Needs

### Support Agent

The support agent needs to focus on critical tickets first, identify urgent issues from ticket descriptions, and reduce manual prioritization effort.

### Customer

The customer needs urgent issues to be resolved quickly because critical issues may otherwise wait behind lower-priority tickets.

### Support Manager

The support manager needs to assign tickets to the appropriate support level and reduce SLA-related risk.

## 6. Selected Solution

Several approaches were considered:

- Manual ticket registers
- Spreadsheet-based ticket tracking
- Standalone support portal
- Salesforce CRM with Agentforce

**Selected Solution: Salesforce CRM with Agentforce**

Salesforce was selected because it provides:

- Automation through Salesforce Flow
- AI-assisted ticket analysis through Agentforce
- Automatic priority classification
- Automatic urgent-task creation
- Reduced manual support effort

## 7. Customer Journey

The proposed customer-support journey is:

```text
Ticket Creation
      ↓
Account Identification
      ↓
Ticket Retrieval
      ↓
Description Analysis
      ↓
Priority Classification
      ↓
Agent Assignment
      ↓
Urgent Task Creation
      ↓
Support Handling
      ↓
Resolution

8. Functional Requirements
ID	Requirement
FR-1	Support ticket creation and management
FR-2	Account and Contact association
FR-3	Ticket description analysis
FR-4	Automatic priority classification
FR-5	High-priority task creation
FR-6	Support agent assignment
FR-7	SLA breach risk checking
FR-8	Agentforce conversational analysis
9. Non-Functional Requirements
Requirement	Description
Usability	Clear Agentforce interaction and understandable priority messages
Reliability	Consistent priority classification based on configured conditions
Performance	Quick retrieval and analysis of the latest support ticket
Security	Controlled Salesforce access to customer and ticket information
Maintainability	Flow and Agentforce configuration can be updated
Scalability	Supports increasing support-ticket volumes through Salesforce automation
10. Data Flow
Flow
Customer / Support Request
        ↓
Account
        ↓
Latest Support Ticket
        ↓
Ticket Description
        ↓
Analyze Description
        ↓
Priority Level
        ↓
High / Medium / Low Decision
        ↓
Agent Assignment
        ↓
High-Priority Task (if applicable)
        ↓
Final Response
Agentforce Flow
User provides Account Name
        ↓
Agentforce Support Ticket Priority Analysis
        ↓
Auto-Launched Flow
        ↓
Ticket Analysis
        ↓
Priority and Assignment Output
        ↓
Response to User
11. Technology Stack
Layer	Technology
Platform	Salesforce
Database	Support_Ticket_Intelligence__c with Account, Contact, User and Task records
Logic	Auto-Launched Flow
AI	Agentforce
Agent Configuration	Support Ticket Priority Analysis subagent
Automation	Flow Decision, Assignment and Create Records
Output	Agentforce conversation response
12. High-Level Solution Architecture
Users / Support Team
        ↓
Agentforce
        ↓
Support Ticket Priority Analysis
        ↓
Auto-Launched Flow
        ↓
Account & Ticket Data
        ↓
Description Analysis
        ↓
Priority Decision
        ↓
Task / Assignment
        ↓
Final Message
        ↓
Agentforce Response


## 13. Priority Classification Logic

The system uses configured urgency keywords to classify ticket priority.

High Priority

If the ticket description contains:

urgent
not working
failure

Priority:

High

Medium Priority

If the description contains:

issue
slow
delay

Priority:

Medium

Low Priority

If none of the configured High- or Medium-priority keywords are found:

Priority:

Low

14. Proposed Business Outcome

The solution is intended to provide:

Faster identification of critical tickets.
Reduced manual ticket-processing effort.
More consistent support assignment.
Automatic handling of urgent tickets.
Better awareness of SLA-related risk.
AI-assisted conversational ticket analysis.


## 15. Project Scope
In Scope
Salesforce-based support ticket management
Support Ticket Intelligence custom object
Account and Contact association
Ticket description analysis
Automatic priority classification
Support-agent assignment
High-priority task creation
Auto-Launched Flow
Agentforce-based ticket analysis
SLA breach risk checking
Conversational output
Future / Optional Enhancements
Additional assignment rules
More advanced SLA monitoring
Additional priority indicators
Expanded support teams
More advanced analytics
16. Expected Workflow
Support Ticket Created
        ↓
Retrieve Customer Account
        ↓
Retrieve Latest Support Ticket
        ↓
Analyze Ticket Description
        ↓
Determine Priority
        ↓
Assign Appropriate Support Level
        ↓
Create Urgent Task for High Priority
        ↓
Return Result to Agentforce


## 17. Conclusion

The Customer Support Ticket Priority Prediction and Automated Assignment System demonstrates how Salesforce Flow and Agentforce can work together to reduce manual support-ticket handling.

The proposed solution analyzes ticket descriptions, determines priority as High, Medium, or Low, assigns the appropriate support level, and creates an urgent handling task for High-priority tickets. This establishes the foundation for the project's subsequent Salesforce object, Flow, Agentforce, assignment, and testing activities.


This version is much closer to the actual document. In particular, the document defines **FR-1 to FR-8**, the specific **customer journey**, the **Salesforce/Agentforce stack**, and the keyword logic; those should be reflected in your repository rather than adding requirements that aren't in the reference. :contentReference[oaicite:2]{index=2} :contentReference[oaicite:3]{index=3} :contentReference[oaicite:4]{index=4}

One more important correction: **don't describe advanced analytics as part of the current implementation**. The document explicitly treats advanced analytics for priority trends and resolution performance as future scope. :contentReference[oaicite:5]{index=5}

Also, your **Milestone 2 object fields are explicitly defined in this PDF**, so when you reach that task, we should follow these exact fields rather than the simplified list from the earlier SkillWallet description. The document specifies `Ticket Number`, `Customer`, `Contact`, `Issue Type`, `Description`, `Priority Level`, `Status`, `Created Date`, `Assigned To`, `SLA Breach Risk`, and `Resolution Time`. :contentReference[oaicite:6]{index=6}

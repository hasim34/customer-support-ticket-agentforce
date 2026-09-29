# Phase 1: Gathering & Analyzing User Needs

## 1. Purpose

This activity identifies the key users of the Customer Support Ticket Priority Prediction and Automated Assignment System and analyzes their needs, expectations, pain points, and interactions with the proposed Salesforce and Agentforce solution.

The objective is to understand how different users interact with support tickets and translate their needs into functional and system requirements.

---

## 2. Users Involved

The proposed system involves the following primary users and system component:

| User / Component | Role |
|---|---|
| Support Agent | Handles customer support tickets and focuses on urgent issues |
| Support Manager | Monitors ticket workload, status, priority, and support handling |
| Customer | Raises support issues and expects timely resolution |
| AI Agent (Agentforce) | Analyzes ticket information and triggers backend automation |

---

## 3. Support Agent Needs

### Role

Support agents are responsible for reviewing and handling customer support tickets, focusing on important issues, and resolving assigned tickets.

### Current Needs

Support agents need to:

- Identify critical tickets quickly.
- Understand the priority of each ticket.
- See ticket status and issue information clearly.
- Receive tickets appropriate to their support level.
- Avoid spending unnecessary time manually determining ticket priority.
- Ensure urgent customer issues receive timely attention.

### Pain Points

The project analysis identifies the following pain points for support agents:

- Manual ticket prioritization.
- Difficulty identifying urgent issues from descriptions.
- Important urgency signals may be missed.
- Delays in handling critical customer issues.
- Manual decisions when determining which tickets require immediate attention.

### Expected Gains

The proposed system should provide support agents with:

- Automatic priority classification.
- Clearer ticket assignment.
- Automatic urgent-task creation for High-priority tickets.
- Reduced manual prioritization effort.
- Faster identification of critical issues.

---

## 4. Support Manager Needs

### Role

The support manager is responsible for monitoring support operations, assigning tickets to the appropriate support level, and maintaining visibility into ticket handling.

### Current Needs

Support managers need to:

- Monitor support-ticket workload.
- Understand ticket priority and status.
- Assign tickets to appropriate support levels.
- Identify older unresolved tickets.
- Be aware of potential SLA-related risk.
- Monitor how support tickets are being handled.

### Pain Points

The project analysis identifies:

- Manual assignment decisions.
- Difficulty consistently analyzing ticket urgency.
- Large ticket volumes.
- Potential SLA-related risk from older unresolved tickets.
- Need for additional checking of unresolved tickets.

### Expected Gains

The proposed system should provide:

- More consistent support assignment.
- Automatic priority classification.
- Visibility into ticket information.
- High-priority task creation.
- SLA-risk awareness.
- Reduced manual handling effort.

---

## 5. Customer Needs

### Role

Customers create support requests and provide descriptions of their problems.

### Customer Needs

Customers expect:

- Their support issue to be recorded correctly.
- Urgent issues to receive attention quickly.
- Their tickets to be prioritized appropriately.
- Their issues to be handled by the appropriate support level.
- Faster response and resolution.

### Customer Pain Point

A critical customer issue may wait behind lower-priority tickets when support teams handle large ticket volumes manually.

### Expected Gain

Automatic analysis and prioritization should help critical customer issues receive attention more quickly.

---

## 6. Agentforce Needs / System Role

Agentforce is used as the AI component of the solution.

### Responsibilities

Agentforce should:

- Analyze customer support ticket information.
- Analyze the ticket description.
- Determine ticket priority.
- Assist with support-ticket prioritization.
- Trigger the backend automation through the configured Flow.
- Return a clear action-oriented response.

### Agentforce Interaction

The documented system flow is:

```text
User provides Account Name
        ↓
Agentforce
        ↓
Support Ticket Priority Analysis
        ↓
Auto-Launched Flow
        ↓
Ticket Analysis
        ↓
Priority and Assignment Output
        ↓
Response to User
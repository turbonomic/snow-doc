---
layout: default
title: Action Approval States and Transitions
---
The valid action states in {{ site.data.vars.Product_Short }}  and 
the {{ site.data.vars.Product_Short }} Actions application are:

| **{{ site.data.vars.Product_Short }} Action State** | **Action Approval States** |
| --- | --- |
| PENDING_ACCEPT | PENDING_APPROVAL |
| ACCEPTED | APPROVED |
| REJECTED | REJECTED |
| QUEUED | WAITING_FOR_EXEC |
| RECOMMENDED | WAITING_FOR_CR_SCHEDULE |
| IN_PROGRESS | IN_PROGRESS |
| SUCCEEDED | SUCCEEDED |
| FAILED | FAILED |
| CLEARED | MISSED |
| DISABLED |  |

The states and their transitions are as follows:


{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% else %}
<img src="{{ '/assets/SNOW_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% endif %}

The initial state of each action approval is PENDING_APPROVAL. From that state an approval will move to one of the following ones:

- `APPROVED`
  The CR generated for the action approval has been approved (e.g. its state is 
  `SCHEDULED` or `IMPLEMENT`) and its schedule is on, or there is no schedule configured 
  for that change request.
  
- `WAITING_FOR_CR_SCHEDULE`
  The corresponding CR has been approved and has a future schedule associated to it.

- `REJECTED`
  The CR referred by the action approval has been canceled.

- `MISSED`
  {{ site.data.vars.Product_Short }} no longer recommends this action. {{ site.data.vars.Product_Short }} 
  hasn't recommended the action 
  for a period longer than [`Last_Recommended_Time` + `Action_Retention_Time`], where:
  * `Last_Recommended_Time` = the last time the action was recommended by {{ site.data.vars.Product_Short }};
  * `Action_Retention_Time` = the Action Retention setting (1 day by default);

From the `APPROVED` state, the action approval can transition into one of the following states:

- `WAITING_FOR_CR_SCHEDULE`
  The action's corresponding CR has a schedule associated to it and that schedule is off.

- `WAITING_FOR_EXEC`
  {{ site.data.vars.Product_Short }} has scheduled the approved action for execution.

- `IN_PROGRESS`
  {{ site.data.vars.Product_Short }} has started execution of the action.

- `SUCCEEDED`
  The approved action has been successfully executed by {{ site.data.vars.Product_Short }}.

- `FAILED`
  The approved action failed its execution in {{ site.data.vars.Product_Short }}.

- `MISSED`
  {{ site.data.vars.Product_Short }} no longer recommends this action. {{ site.data.vars.Product_Short }} 
  hasn't recommended the action 
  for a period longer than [`Last_Recommended_Time` + `Action_Retention_Time`], where:
  * `Last_Recommended_Time` = the last time the action was recommended by {{ site.data.vars.Product_Short }};
  * `Action_Retention_Time` = the Action Retention setting (1 day by default);

From the `WAITING_FOR_CR_SCHEDULE` state, an approval can transition into one of the following states:

- `APPROVED`
  The approval's corresponding CR has a schedule associated to, it and that schedule is on.

- `MISSED`
  {{ site.data.vars.Product_Short }} no longer recommends this action. {{ site.data.vars.Product_Short }} 
  hasn't recommended the action 
  for a period longer than [`Last_Recommended_Time` + `Action_Retention_Time`], where:
  * `Last_Recommended_Time` = the last time the action was recommended by {{ site.data.vars.Product_Short }};
  * `Action_Retention_Time` = the Action Retention setting (1 day by default);

From the `WAITING_FOR_EXEC` state, an approval can transition into one of the following states:

- `IN_PROGRESS`
  {{ site.data.vars.Product_Short }} has started execution of the action.

- `SUCCEEDED`
  The approved action has been successfully executed by {{ site.data.vars.Product_Short }}.

- `FAILED`
  The approved action failed its execution in {{ site.data.vars.Product_Short }}.

- `MISSED`
  {{ site.data.vars.Product_Short }} no longer recommends this action. {{ site.data.vars.Product_Short }} 
  hasn't recommended the action 
  for a period longer than [`Last_Recommended_Time` + `Action_Retention_Time`], where:
  * `Last_Recommended_Time` = the last time the action was recommended by {{ site.data.vars.Product_Short }};
  * `Action_Retention_Time` = the Action Retention setting (1 day by default);

From the `IN_PROGRESS` state, an action approval can transition to one of the following states:

- `SUCCEEDED`
  The approved action has been successfully executed by {{ site.data.vars.Product_Short }}.

- `FAILED`
  The approved action failed its execution in {{ site.data.vars.Product_Short }}.


---

**NOTE:**

For reference, this diagram shows the action states and their transitions in {{ site.data.vars.Product_Short }}:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_T8c_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% else %}
<img src="{{ '/assets/SNOW_T8c_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% endif %}

---

---
layout: default
title: Custom Business Rules
---
By default, the {{ site.data.vars.Product_Short }} Actions app creates corresponding 
change requests in ServiceNow for each action approval. When the change requests states 
do not match those the application expects or when you have complex workflows for 
creating and managing your CRs, you must implement custom custom business rules.

The default change request states used by the {{ site.data.vars.Product_Short }} 
Actions application are:

| Change Request State | Value |
| --- | --- |
| NEW | -5 |
| ASSESS | -4 |
| AUTHORIZE | -3 |
| SCHEDULED | -2 |
| IMPLEMENT | -1 |
| REVIEW | 0 |
| CLOSE | 3 |
| CANCELLED | 4 |

To enable your custom business rules, you must check the 
**Use Custom Business Rule for CR Creation** application setting:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Cust_BR_Enable.png' | relative_url }}" alt="Enabling Custom Business Rules" width="950">
{% else %}
<img src="{{ '/assets/SNOW_Cust_BR_Enable.png' | relative_url }}" alt="Enabling Custom Business Rules" width="950">
{% endif %}

Note that your custom business rules must:

- Create a new CR for every newly added action approval record in 
  the `PENDING_APPROVAL` state. It must also link the CR to the action approval.

- For every change request, update the state of its corresponding action approval 
  based on the approval/rejection of the request. When the CR is canceled, the action 
  approval state must be set to `REJECTED`. When the CR is approved, the action approval 
  state must be set to `APPROVED`.

- If a CR is approved but has a future schedule attached to it (Planned Start Date is 
  in the future), the state of its corresponding action approval must be set to 
  `WAITING_FOR_CR_SCHEDULE`. Later on, when the CR schedule is on, the rule must change 
  the approval state to `APPROVED`.

- After the {{ site.data.vars.Product_Short }} action completes its execution and the 
  approval state is set to `SUCCEEDED` or `FAILED`, the rule must close the corresponding 
  CR as successful or unsuccessful, respectively. Similarly, the rule must close the CR as 
  unsuccessful when the action approval transitions to the `MISSED` state.
  
  ---
  **Note:** If your business rules do not implement these logic steps, we cannot guarantee that 
  {{ site.data.vars.Product_Short }} will execute the given actions.
  
  ---
  
  
  
  


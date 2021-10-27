---
layout: default
title: Collaboration Diagram
---

This diagram illustrates the main components of {{ site.data.vars.Product_Short }} 
Actions, and the component interactions:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Collaboration_Diagram.png' | relative_url }}" alt="Collaboration Diagram">
{% else %}
<img src="{{ '/assets/SNOW_Collaboration_Diagram.png' | relative_url }}" alt="Collaboration Diagram">
{% endif %}




The {{ site.data.vars.Product_Short }} configures the ServiceNow target and then configures automation 
policies to send specific actions to ServiceNow for approval. For any actions that are configured for 
approval, the default action approval workflow is:

1. The ServiceNow probe periodically uses REST API calls to send actions for approval to the {{ site.data.vars.Product_Short }} 
   Actions application (running on ServiceNow).

2. The {{ site.data.vars.Product_Short }} 
   Actions application creates action approvals for each {{ site.data.vars.Product_Short }} 
   action.  By default, it creates corresponding Change Requests (CRs) to be approved by the ServiceNow administrator:

    a. If a change request is approved for a future date/time (Planned Start Date and Planned End Date), 
    the action will be executed in {{ site.data.vars.Product_Short }} according to that CR schedule.
    
    b. If the change request is approved right away, the action returns to {{ site.data.vars.Product_Short }} 
    immediately for execution. If is no execution schedule in {{ site.data.vars.Product_Short }} for that 
    that action, it executes immediately.  If there is an execution schedule in {{ site.data.vars.Product_Short }}, 
    the action executes according to that schedule.
    
    c. For a rejected change request, the corresponding actions does not execute. In addition, for one week (by default), 
    {{ site.data.vars.Product_Short }} will not request new approvals for the same action.
    
    d. If an action is no longer recommended by {{ site.data.vars.Product_Short }}, the approval is marked as _missed_ in 
    ServiceNow after a 24 hour period.

3. The ServiceNow probe periodically polls for action approvals states. If it discovers any approved actions, 
   {{ site.data.vars.Product_Short }} schedules them for execution.

4. After an action completes its execution, the ServiceNow probe updates the approval state as 
   `SUCCEEDED` or `FAILED` in ServiceNow. In addition, it directs ServiceNow to close 
   the corresponding change request as successful or unsuccessful.


---
**NOTE:** 

If you use custom business rules, the integration does not create CRs in ServiceNow. 
You must create the CRs in your business rules, and the CRs mmust refer to the proper action approvals. 
When a CR is approved or rejected, the rules must set the action approval state to `APPROVED` or 
`REJECTED`, respectively. After {{ site.data.vars.Product_Short }} executes the action and sets the 
approval state to `SUCCEEDED` or `FAILED`, your custome business rules must close the 
give CR as successful or unsuccessful.

---

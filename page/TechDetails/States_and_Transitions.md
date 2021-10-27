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


{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_T8c_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% else %}
<img src="{{ '/assets/SNOW_T8c_Action_State_Diagram.png' | relative_url }}" alt="Action State Diagram">
{% endif %}


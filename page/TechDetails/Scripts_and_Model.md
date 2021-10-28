---
layout: default
title: Server Scripts and Data Model
---

The {{ site.data.vars.Product_Short }} Actions application includes 
a number of scripts. You can identify them by the filename prefix, 
`{{ site.data.vars.Product_Short }}`.


## Request Processor

The processor logic is implemented in the 
`TurbonomicRequestProcessor` server script. It has dedicated methods for:

- Creating action records.

- Creating, updating, and searching for action approvals.

- Getting and creating entries in the {{ site.data.vars.Product_Short }} Entity table.

- Getting and updating entries in the {{ site.data.vars.Product_Short }} Instance table.

The request processor uses the services of `TurbonomicEntityMatcher` for finding the 
The configuration items of the corresponding {{ site.data.vars.Product_Short }} entities 
and `TurbonomicActionApprovalManager` for action reconciliation purposes. 

All data is 
written to the application tables via the data access object scripts: 
`TurbonomicActionDAO` and `TurbonomicChangeRequestDAO`. These scripts rely 
on the import set mechanism to insert and update the table records.

## Action Approval Manager

The action Approval Manager is implemented in the `TurbonomicActionApprovalManager` script. 

This module adds and updates action approval records in ServiceNow, based on the input set of 
{{ site.data.vars.Product_Short }} actions that require external approval. When a new action 
approval record is created, a corresponding change request is also generated for it (if 
custom business rules are not in use). The ServiceNow administrator will approve or cancel 
the change request and that will automatically update the state of its matching action approval. 
{{ site.data.vars.Product_Short }} then decides whether to execute the given action, based on 
the `APPROVED` or `REJECTED` state of the action approval record found in ServiceNow.

Another major responsibility of the action approval manager is to perform action 
reconciliation, based on the {{ site.data.vars.Product_Short }} action state and the state 
of the action's corresponding action approval record in ServiceNow.

## Entity Matcher

The action Entity Manager is implemented in the `TurbonomicEntityMatcher` script. 

This module serves to find the corresponding Configuration Item (CI) in ServiceNow for a given 
{{ site.data.vars.Product_Short }} entity. Currently, the {{ site.data.vars.Product_Short }} Actions 
application has matching capabilities for virtual machines hosted in vCenter, Hyper-V, AWS or 
Azure environments. We reserve the right to implement additional entity matching logic 
in future application releases.

---
**Note:**

If the default CI matching mechanism doesn't work on the customer environment, you can implement 
a custom business rule for that purpose. For an example custom rule,
see [APPENDIX Implementing Custom CI Matching](/page/TechDetails/Custom_CI_Matching.html).

---

## Data Access Objects

This module accesses data in the {{ site.data.vars.Product_Short }} Actions application, and 
in ServiceNow.

The `TurbonomicActionDAO` script reads and writes records in tables that are installed as part 
of the {{ site.data.vars.Product_Short }} Actions application.  The `TurbonomicChangeRequestDAO` 
script reads and writes records in the `change_request` table that is managed by ServiceNow. 
When loading and storing this data, the module uses the following scripts:

- `TurbonomicActionApproval` -- stores records from the {{ site.data.vars.Product_Short }} Action Approval table.

- `TurbonomicActionApprovalState` -- stores all possible action approval states that are defined in the {{ site.data.vars.Product_Short }} Action State table.

- `TurbonomicActionRecord` -- stores record data from the {{ site.data.vars.Product_Short }} Action Record table.

- `TurbonomicChangeRequest` -- stores change request data required by the {{ site.data.vars.Product_Short }} action application.

- `TurbonomicChangeRequestState` -- defines the supported change request states.

- `TurbonomicEntity` -- stores the record data from the {{ site.data.vars.Product_Short }} Entity table.

- `TurbonomicInstance` -- stores the records data from the {{ site.data.vars.Product_Short }} Instance table.

## Data Model

The data model for {{ site.data.vars.Product_Short }} Actions is represented by a number of 
tables.  The most important of these are:

- {{ site.data.vars.Product_Short }} Action Approval (`x_turbo_turbonomic_turbonomic_action_approval`)

- {{ site.data.vars.Product_Short }} Action Record (`x_turbo_turbonomic_turbonomic_action_record`)

- {{ site.data.vars.Product_Short }} Action Settings (`x_turbo_turbonomic_turbonomic_settings`)

- {{ site.data.vars.Product_Short }} Action State (`x_turbo_turbonomic_turbonomic_action_state`)

- {{ site.data.vars.Product_Short }} Entity (`x_turbo_turbonomic_x_turbonomic_entity`)

- {{ site.data.vars.Product_Short }} Instance (`x_turbo_turbonomic_turbonomic_instance`)

Note that we have implemented matching import set tables to work with these tables. 
This enables the application to add or update the table data.











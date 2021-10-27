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

The action approval manager is implemented in the `TurbonomicActionApprovalManager` script. 

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



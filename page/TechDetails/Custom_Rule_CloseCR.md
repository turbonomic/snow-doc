---
layout: default
title: EXAMPLE Close CR
---

As part of executing actions, {{ site.data.vars.Product_Short }} connects to 
ServiceNow to update the action approval states.  It can set them to:

- `IN_PROGRESS`
- `FAILED`
- `SUCCEEDED`
- `MISSED`

If you use custom business rules to create CRs, then you must also use custom business 
If rules to close the CRs that you have created. You can close a CR as successful or unsuccessful, 
depending on the action approval state. To do this, you create a business rule that runs for a CR 
when the application changes the state of the corresponding {{ site.data.vars.Product_Short }} 
Action Approval table entry.

To create this rule, navigate to 
**System Definition > Business Rules** and create a rule for the 
{{ site.data.vars.Product_Short }} Actions Approval table that is similar to the following:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Cust_BR_CloseCR.png' | relative_url }}" alt="CI Matching">
{% else %}
<img src="{{ '/assets/SNOW_Cust_BR_CloseCR.png' | relative_url }}" alt="CI Matching">
{% endif %}


### Main Fields:

| Field Name | Value |
| --- | --- |
| Name | Custom Matching Change Requests |
| Table | {{ site.data.vars.Product_Short }} Action Approval [x_turbo_turbonomic_turbonomic_action_approval] |
| Application | {{ site.data.vars.Product_Short }} Actions |
| Active | ON |
| Advanced | ON |

### When to run:

| Field Name | Value |
| --- | --- |
| When | after |
| Order | 100 |
| Insert | OFF |
| Update | ON |
| Delete | OFF |
| Query | OFF |

For the other fields, use the defaults.

### Advanced:

Add JavaScript code similar to the following, where you close the CR based on the 
action approval state:

<pre>
(function executeRule(current, previous /*null when async*/) {

    var changeRequest = null;
    var dao = new x_turbo_turbonomic.TurbonomicActionDAO();
    var actionApprovalState = dao.getActionStateById(current.state_id);

    switch (actionApprovalState) {
        case 'FAILED':
        case 'MISSED':
            changeRequest = new GlideRecordSecure('change_request');
            changeRequest.addQuery('sys_id', current.change_request_id);
            changeRequest.query();

            if (changeRequest.next()) {
                changeRequest.state = '3'; // Default change request closed state
                changeRequest.close_code = 'unsuccessful';
                changeRequest.work_notes = 'Matching CR for the failed Turbonomic action has been closed as unsuccessful';

                // TODO: Update additional change request fields, as required
                changeRequest.close_notes = '...';

                changeRequest.update();
            }
            break;

        case 'SUCCEEDED':
            changeRequest = new GlideRecordSecure('change_request');
            changeRequest.addQuery('sys_id', current.change_request_id);
            changeRequest.query();

            if (changeRequest.next()) {
                changeRequest.state = '3'; // Default change request closed state
                changeRequest.close_code = 'successful';
                changeRequest.work_notes = 'Matching CR for the succeeded Turbonomic action has been closed as successful';

                // TODO: Update additional change request fields, as required
                changeRequest.close_notes = '...';

                changeRequest.update();
            }
            break;

        default:
            gs.debug('Change request changes are not required for Turbonomic action approval');
    }

})(current, previous);
</pre>
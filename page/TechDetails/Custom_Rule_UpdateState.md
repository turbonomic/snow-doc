---
layout: default
title: EXAMPLE Update Action Approval State
---

For any action associated with a CR, {{ site.data.vars.Product_Short }} periodically checks the 
state values for the associated action approval record in ServiceNow. The ServiceNow 
administrator can create a custom business rule to set the approval state to one of:

- `APPROVED`
- `REJECTED`

Before you can use this custom rule, you must turn off **Active** for the 
`UpdateApprovalBasedOnRequestStateChanges` default business rule:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Cust_BR_Disable_Default_Update.png' | relative_url }}" alt="CI Matching">
{% else %}
<img src="{{ '/assets/SNOW_Cust_BR_Disable_Default_Update.png' | relative_url }}" alt="CI Matching">
{% endif %}

To create this rule, navigate to 
**System Definition > Business Rules** and create a rule for the 
Change Request table that is similar to the following:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Cust_BR_ChangeState.png' | relative_url }}" alt="CI Matching">
{% else %}
<img src="{{ '/assets/SNOW_Cust_BR_ChangeState.png' | relative_url }}" alt="CI Matching">
{% endif %}


### Main Fields:

| Field Name | Value |
| --- | --- |
| Name | Custom State Updates of Action Approvals |
| Table | Change Request [change_request] |
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

Add JavaScript code similar to the following, where you determine when to set the state to `APPROVED` 
or `REJECTED`:

<pre>
(function executeRule(current, previous /*null when async*/) {

    var actionApproval = new GlideRecordSecure(
                                'x_turbo_turbonomic_turbonomic_action_approval');
    actionApproval.addQuery('change_request_id', current.sys_id);
    actionApproval.query();

    if (actionApproval.next()) {
        var dao = new x_turbo_turbonomic.TurbonomicActionDAO();
        var actionStateId = '';
        var changeRequestState = current.state + '';
        var actionApprovalState = dao.getActionStateById(actionApproval.state_id);

        switch (changeRequestState) {
            case CUSTOM_CR_APPROVED_STATE:
                if (actionApprovalState == 'PENDING_APPROVAL') {
                    actionStateId = dao.getActionStateId('APPROVED');
                    actionApproval.setValue('state_id', actionStateId);
                    actionApproval.update();
                }
                break;

            case CUSTOM_CR_CANCELLED_STATE:
                if (actionApprovalState == 'PENDING_APPROVAL') {
                    actionStateId = dao.getActionStateId('REJECTED');
                    actionApproval.setValue('state_id', actionStateId);
                    actionApproval.update();
                }
                break;

            default:
                gs.debug('Turbonomic action approval state changes are not required for the change request');
        }
    }

})(current, previous);
</pre>
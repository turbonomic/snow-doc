---
layout: default
title: EXAMPLE Create CR
---

To customize how {{ site.data.vars.Product_Short }} Actions creates CRs, the ServiceNow 
administrator can define a custom business rule. Navigate to 
**System Definition > Business Rules** and create a rule for the 
{{ site.data.vars.Product_Short }} Actions Approval table that is similar to the following:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_Cust_BR_CreateCR.png' | relative_url }}" alt="CI Matching">
{% else %}
<img src="{{ '/assets/SNOW_Cust_BR_CreateCR.png' | relative_url }}" alt="CI Matching">
{% endif %}

Define this rule to run when {{ site.data.vars.Product_Short }} Actions inserts a new record into the 
{{ site.data.vars.Product_Short }} Action Approval table. Turn **Advanced** on so you can 
include custom JavaScript code.

### Main Fields:

| Field Name | Value |
| --- | --- |
| Name | Create Matching CR for Action Approval |
| Table | {{ site.data.vars.Product_Short }} Action Approval [x_turbo_turbonomic_turbonomic_action_approval] |
| Application | {{ site.data.vars.Product_Short }} Actions |
| Active | ON |
| Advanced | ON |

### When to run:

| Field Name | Value |
| --- | --- |
| When | before |
| Order | 100 |
| Insert | ON |
| Update | OFF |
| Delete | OFF |
| Query | OFF |

For the other fields, use the defaults.

### Advanced:

Add JavaScript code similar to the following, where you specify the CR values that you want 
the rule to populate:

<pre>
(function executeRule(current, previous /*null when async*/) {
 
    var changeRequest = new GlideRecordSecure("change_request");
    changeRequest.newRecord();

    changeRequest.type = 'standard';
    changeRequest.start_date = gs.daysAgo(-1);
    changeRequest.end_date = gs.daysAgo(-2);
    changeRequest.description = current.description;
    // TODO: Set additional change requests fields, as required

    // Replace with valid sys id of a standard change producer version record
    var chgProducerVersion = "d08ec5d9dbXXXXXXXXXXXXXXXXXXXXXXXX";

    var chgTemplate = new GlideRecordSecure("std_change_producer_version");
    chgTemplate.get("sys_id", chgProducerVersion);

    var template = GlideTemplate.get(chgTemplate.std_change_producer.template);
    template.apply(changeRequest);

    changeRequest.work_notes = "CR has been created using the template named: " +
           chgTemplate.std_change_producer.name;
     
    // Links the Action Approval entry to the newly created Change Request record
    current.change_request_id = changeRequest.insert();
 
})(current, previous);
</pre>
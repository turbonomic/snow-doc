---
layout: default
title: REST API
---

To communicate with the {{ site.data.vars.Product_Short }} Actions application from an 
external application, use the {{ site.data.vars.Product_Short }} Actions REST API. 
You can execute these operations:

- Create Action Records (POST)

- Create Action Approvals (POST)

- Update Action Approvals (POST)

- Search Action Approvals (PUT)

Note that API request payloads and the API responses are in jSON format.

## Create Action Records

Adds new entries into the {{ site.data.vars.Product_Short }} Actions records table. Use this for 
{{ site.data.vars.Product_Short }} actions that have been recommended or have already been executed.


**API Request:** 

`POST /api/x_turbo_turbonomic/v1/action/records`

**Sample Body:**

<pre>
{
    "turbonomicInstance": 
    {
        "hostOrIP": "10.10.200.80",
        "status": "ONLINE",
        "version": "6.4.0-SNAPSHOT"
    },
    "actionRecords": [
        {
            "actionOid": "A1234567890",
            "details": "Hyper-V VM 1 Right Size Action Details",
            "targetEntity": 
            {
                "uuid": "vm-345",
                "name": "ACM-Hyper-V-VM-1",
                "type": "VIRTUAL_MACHINE",
                "targetName": "10.10.172.10",
                "targetIP": "10.10.172.10",
                "targetType": "hyper-v"
            },
            "actionLifeCycleEvent": "On Generation"
            … // Extra fields removed

        },
  	 {
            "actionOid": "B1234567890",
            "details": "vCenter VM 2 Right Size Action Details",
            "targetEntity": 
            {
                "uuid": "vm-367",
                "name": "ACM-L-Tomcat-VM-2",
                "type": "VIRTUAL_MACHINE",
                "targetName": "10.10.172.25",
                "targetIP": "10.10.172.25",
                "targetType": "vCenter"
            },
            "actionLifeCycleEvent": "On Generation"
            … // Extra fields removed
        }
    ]
}
</pre>

**Sample Response:**

<pre>
{
    "result": {
        "succeeded": [
            {
                "oid": "A1234567890"
            },
            {
                "oid": "B1234567890"
            }
        ],
        "failed": []
    }
}
</pre>



## Create Action Approvals

Create {{ site.data.vars.Product_Short }} action approval records in ServiceNow for each 
{{ site.data.vars.Product_Short }} that requires approval. By default, this creates 
a corresponding CR.


**API Request:** 

`POST /api/x_turbo_turbonomic/v1/action/approvals`

**Sample Body:**

<pre>
{
    "turbonomicInstance": 
    {
        "hostOrIP": "10.10.200.80",
        "status": "ONLINE",
        "version": "6.4.0-SNAPSHOT"
    },
    "actionApprovals": [
        {
            "oid": "A1234567890",
            "description": "vCenter VM 1 Right Size Action Description",
            "category": "Efficiency",
            "commodityName": "VMEM",
            "from": “2GB",
            "to": "1GB",
            "risk": "Action Risk Description",
            "savings": 123.45,
            "state": "PENDING_ACCEPT",
            "type": "RIGHT_SIZE",
            "timestampMsec": 123456789,
            "targetEntity":
            {
                "uuid": "vm-367",
                "name": "ACM-L-Tomcat1-172.25",
                "type": "VIRTUAL_MACHINE",
                "targetName": "vsphere-dc1.corp",
                "targetType": "vCenter"
            }
        }
        … // Extra fields removed
    ]
}
</pre>

**Sample Response:**

<pre>
{
    "result": {
        "succeeded": [
            {
                "oid": " A1234567890",
                "changeRequestId": "eda81daedbcbb3005e691fe9689619e1",
                "changeRequestNumber": "CHG0049469",
                "state": "PENDING_APPROVAL"
            }
        ],
        "failed": []
    }
}
</pre>


## Update Action Approvals

Modify existing action approval records that were created by {{ site.data.vars.Product_Short }} Actions.


**API Request:** 

`PUT /api/x_turbo_turbonomic/v1/action/approvals`

**Sample Body:**

<pre>
{
    "turbonomicInstance": 
    {
        "hostOrIP": "10.10.200.80",
        "status": "ONLINE",
        "version": "6.4.0-SNAPSHOT"
    },
    "actionApprovalUpdates": [
        {
            "oid": "A1234567890",
            "description": "vCenter VM 1 Right Size Action Description",
            "category": "Efficiency",
            "commodityName": "VMEM",
            "from": “2GB",
            "to": "1GB",
            "risk": "Action Risk Description",
            "savings": 123.45,
            "state": "PENDING_ACCEPT",
            "type": "RIGHT_SIZE",
            "timestampMsec": 123456789,
            "targetEntity":
            {
                "uuid": "vm-367",
                "name": "ACM-L-Tomcat1-172.25",
                "type": "VIRTUAL_MACHINE",
                "targetName": "vsphere-dc1.corp",
                "targetType": "vCenter"
            }
        }
    ]
}
</pre>

**Sample Response:**

<pre>
{
    "result": {
        "succeeded": [
            {
                "oid": " A1234567890",
                "changeRequestId": "eda81daedbcbb3005e691fe9689619e1",
                "changeRequestNumber": "CHG0049469",
                "state": "PENDING_APPROVAL"
            
        ],
        "failed": []
    }
}
</pre>


## Search Action Approvals

Find a given {{ site.data.vars.Product_Short }} action approval record in ServiceNow.

You can include the `addAllApprovalsInTransition` parameter in the Search call. If you set it to 
`true`, the search result will include all approvals that are in one of these states:

* `APPROVED`
* `IN_PROGRESS`
* `WAITING_FOR_EXEC`
* `WAITING_FOR_CR_SCHEDULE`


**API Request:** 

`POST /api/x_turbo_turbonomic/v1/action/search/approvals?addAllApprovalsInTransition=false`

**Sample Body:**

<pre>
{
    "turbonomicInstance": 
    {
        "hostOrIP": "10.10.200.80",
        "status": "ONLINE",
        "version": "6.4.0-SNAPSHOT"
    },
      "oids": 
    [
        "A1234567890”, "B1234567890”
    ]
}
</pre>

**Sample Response:**

<pre>
{
    "result": {
        "succeeded": [
            {
                "oid": " A1234567890",
                "state": "PENDING_APPROVAL"
            },
            {
                "oid": " B1234567890",
                "state": "PENDING_APPROVAL"
            }
        ],
        "failed": []
    }
}
</pre>



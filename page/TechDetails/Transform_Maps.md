---
layout: default
title: Transform Maps
---

The {{ site.data.vars.Product_Short }} Actions application  uses different _transform maps_ to 
perform insert operations. Each transform map name begins with the 
{{ site.data.vars.Product_Short }} prefix.

---
**Note:** If you use custom business rules, then the application does not invoke the 
Turbonomic Web Service Change Request transform map.

---

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_TransformMap_List.png' | relative_url }}" alt="Transform Maps">
{% else %}
<img src="{{ '/assets/SNOW_TransformMap_List.png' | relative_url }}" alt="Transform Maps">
{% endif %}

The default behaviour is to use a 1:1 mapping between the input fields and the table ouput 
columns. If you need to store a different set of data in one or more of the target tables, 
Then we recommend that you:

- Disable the default {{ site.data.vars.Product_Short }} Actions transform map for the target table.

- Create a new transform map with the same input source as the inactive transform map.

- Specify the custom mapping logic, for the table fields of the new transform map.

Below, we show an example that sets a different configuration item than the default 
VirtualMachine instance, in the change request table. The first step is to mark the 
**Turbonomic Web Service Change Request** transform map as inactive:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_TransformMap_CR_Inactive.png' | relative_url }}" alt="Transform Map with CR inactive">
{% else %}
<img src="{{ '/assets/SNOW_TransformMap_CR_Inactive.png' | relative_url }}" alt="Transform Map with CR inactive">
{% endif %}

The next step is to create a Custom Transform Map for the CR. This transform will map all
fields to the same target fields of the change request table, except it does 
_not_ map the Configuration Item (CI) field:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_TransformMap_CustForCR.png' | relative_url }}" alt="Transform Map with custom CR transform">
{% else %}
<img src="{{ '/assets/SNOW_TransformMap_CustForCR.png' | relative_url }}" alt="Transform Map with custom CR transform">
{% endif %}

For the configuration item field, we defined a script to replace the virtual machine with 
its underlying server CI. The example below is for a Hyper-V virtual machine. Similarly, 
the business logic can be modified to address VMware, Azure or AWS virtual machines:

{% if site.github.pages_hostname == "github.io" %}
<img src="{{ site.github.baseurl }}{{ '/assets/SNOW_TransformMap_ReplaceVM.png' | relative_url }}" alt="Transform Map replace VM" width="850">
{% else %}
<img src="{{ '/assets/SNOW_TransformMap_ReplaceVM.png' | relative_url }}" alt="Transform Map replace VM" width="850">
{% endif %}

Note that you can map other source fields to different set target fields as you require.





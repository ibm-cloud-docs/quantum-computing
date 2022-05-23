---
copyright:
  years: 2020, 2022
lastupdated: "2022-04-04"

keywords: security and compliance for *service_name*, security for *service_name*, compliance for *service_name*,

subcollection: # Your subcollection value

---

{{site.data.keyword.attribute-definition-list}}


# Manage security with Qiskit Runtime
{: #manage-security-compliance-qiskit-runtime}


Qiskit Runtime is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}


With the {{site.data.keyword.compliance_short}}, you can:

* Monitor for controls and goals that pertain to *service_name*.
* Define rules for *service_name* that can help to standardize resource configuration.


## Monitoring security and compliance posture with *service_name*
{: #monitor-*service_name*}

As a security or compliance focal, you can use the *service_name* [goals](#x2117978){: term} to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](#x2034950){: term}, you can identify potential issues as they arise.

All of the goals for *service_name* are added to the {{site.data.keyword.cloud_notm}} Best Practices Controls 1.0 profile but can also be mapped to other profiles.
{: note}

To start monitoring your resources, check out [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-monitor-ibm-collector).

### Available goals for *service_name*
{: #*service_name*-available-goals}

* *Check whether certificates are automatically renewed before expiration*
* <additional_goal>



## Governing *service_name* resource configuration
{: #govern-service_name}

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to define configuration rules for the instances of *service_name* that you create.

[Config rules](#x3084914){: term} are used to enforce the configuration standards that you want to implement across your accounts. To learn more about the about the data that you can use to create a rule for *service_name*, review the following table.

| Resource kind | Property | Operator | Value | Description |
|---------------|----------|---------------|-------|-------------|
| *instance* | *private_network_only* | *is_true*  \n *is_false* | - | *Indicates whether access to a Certificate Manager instance is allowed only through a private network.* |
| <resource_kind> | <property_name> | <operator> | <value> | <description> |
{: caption="Table 1. Rule properties for <service_name>" caption-side="bottom"}

To learn more about config rules, check out [What is a config rule?](/docs/security-compliance?topic=security-compliance-what-is-governance).

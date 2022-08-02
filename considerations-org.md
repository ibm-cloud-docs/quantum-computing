---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-02"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m
---

{{site.data.keyword.attribute-definition-list}}


# Considerations for using Qiskit Runtime in an organization
{: #considerations-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

You should understand the following considerations when setting up your environment.
{: shortdesc}


## Auditability
{: #audits-org}

Activity tracker logs significant actions performed on Qiskit Runtime service instances.  Create an instance of Activity Tracker in the region of your Qiskit Runtime instances to get an audit trail of events. Refer to the Qiskit Runtime [Activity Tracker page](https://cloud.ibm.com/docs/quantum-computing?topic=quantum-computing-at_events){: external} for details about the events logged.

This audit log contains the fields `initiator_authnName` and `initiator_authnId`, which match the name shown in [Manage → Access (IAM) → Users](https://cloud.ibm.com/iam/users){: external}.  To view this field, click on the user name, then **Details** in the **IAM ID** field.

![Example of an Activity Tracker event](images/org-guide-audit-example.png "Example of an Activity Tracker event"){: caption="Figure 12. Example of an Activity Tracker event" caption-side="bottom"}

To capture App ID events, open your App ID instance, then navigate to **Manage Authentication -> Authentication settings** and enable **Runtime Activity**.

## Defining more fine grained roles
{: #more-roles-org}

The actions in the custom roles can be used for more fine grained access control. For example, some users might need full access to work on service instances, while others might only need Read access to service instances, programs, and jobs.

To achieve that, define two different custom roles. Remove all cancel, delete, and update roles from the `reader` custom role, and include all actions in the `writer` custom role. Next, add the roles to two different access groups accordingly.

When using dynamic rules, that is, when the IDP administrator manages access through custom IDP user attributes, do not use IDP custom user attributes that are substrings of each other.  For instance, don't use `ml` and `ml-reader`, as the string comparison of `ml` would also accept `ml-reader`. Use `ml-reader` and `ml-writer` to avoid this conflict.
{: note}

## Other Cloud resources
{: #other-cloud-rsc-org}

The steps used in this tutorial can be used to manage access to other Cloud resources as well. Include the appropriate permissions to the access groups of the relevant projects.

## Nested project structures
{: #nest-org}

In this tutorial, the mapping of users to projects and service instances was kept simple. However, by associating several users with access groups and referencing service instances from several access groups, more complex mappings can be implemented.

This method can accommodate a hierarchical structure. That is, it can align to how users might be assigned to the Hub/Group/Project access structure in the IBM Quantum Platform.  For example, a _group_ could be an access group that is assigned to all service instances of the group's projects. Users who should get access to all of the group's projects would then only have to be added to the group's access group.

## Consistent and repeatable deployment of the configuration
{: #repeat-org}

The steps of this tutorial can be automated for consistent and repeatable management of users, projects, and the mapping between those. Refer to the [Terraform IBM Cloud Provider documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external} for templates.

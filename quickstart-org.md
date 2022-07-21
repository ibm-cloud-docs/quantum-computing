---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-21"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Set up Qiskit Runtime in an Organization
{: #quickstart-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

When working in an organization where several individuals might work on several projects, the governance of consuming Qiskit Runtime can become a topic.
Access management can be used to enable collaboration of users working on the same project, as well as to restrict visibility of users and projects that should be isolated from each other.
Managing access becomes particularly relevant when paid Qiskit Runtime resources (i.e. Qiskit Runtime service instances with the non-free Standard plan) are shared.
{: shortdesc}

## overview
{: #overview-org}
{: step}

IBM Cloud provides various ways to implement such mechanism -- this tutorial is one way to achieve these objectives, but may not be the only way.

### Involved Personas
{: #personas-org}
{: step}

The are several main personas used in this tutorial:

* **User**: a user who gets access to Qiskit Runtime resources (service instances) and can potentially collaborate with other users on these resources. Users will only be able to access instances they have been given visibility to. Also, they will not be able to create or delete resources.
* **Cloud administrator**: an owner of an IBM Cloud account owning Qiskit Runtime resources and manages access of other users to these resources. As resource owner, the administrator will also be charged for paid resources.
* **IDP administrator**: an administrator who defines identities and their attributes in an identity provider (IDP).

Other terms used in this tutorial are:

* *Resource*: a generic Cloud term that refers to an object that can be managed through the Cloud UI, CLI or API. In our example of Qiskit Runtime, it refers to a service instance of Qiskit Runtime.
* *Service Instance*: a service instance is used to access Cloud functionality, in our case quantum computing on real devices or simulators. It is defined through the catalog. You can define several service instances based on the same or different plans (offering access to different quantum computing backends). See the [Qiskit Runtime documentation](https://cloud.ibm.com/docs/quantum-computing?topic=quantum-computing-quickstart){: external} for more details.
* *Project*: a grouping unit that is used to enable users to work on the same resources. In that case, users are seen as part of the same project. **Note:** the project is not related to the "project" notion of the IBM Quantum Platform. See the [according section](##nested-project-structures) for more information on this topic.

## Planning
{: #planning-org}
{: step}

The planning process includes decisions on the following aspects:

* decide on how where user identities are defined: are users IBM Cloud users or are users identities in another ID provider?
  * In the latter case, this tutorial will show how to create an ID provider in IBM Cloud
  * note you can have both types of users at the same time if that is really desired
* decide on who manages resource assignments: does the Cloud administrator or the IDP administrator perform assignment of users to project resources?
  * if the IDP administrator performs this assignment, a string will be used as a key for project comparisons. This can be a string like `project`; see the following sections how this string is used (this tutorial uses `project` as this string)
* what are the projects and which service instances should belong to these? Find a name for each project
  * project names should not be substrings of another. E. g. don't use `ml` and `chemlab` because a string match for `ml` will succeed for both strings. Instead use `ml` and `chem-lab`
  * quantum experiments (jobs) belong to service instances and users having access to an instance can see its jobs. Also, service instances can be based on different plans, allowing access to different backends like real devices or simulators -- see the [Qiskit Runtime documentation for more details](https://cloud.ibm.com/docs/quantum-computing?topic=quantum-computing-choose-backend){: external}
* which users should get visibility to which projetcs?
* should users be able to delete jobs?
  * Keeping jobs in service instances gives more traceability to how billing cost was induced.

### Resource Groups
{: #rsc-grp-org}

Instead of access groups directly referencing Qiskit Runtime service instances, service instances can be organized in resource groups.
Service instances can only be created in existing resource groups, and access groups can also only refer to existing resource groups -- therefore resource groups need to be created first.
To do so, go to [Manage -- Account -- Resource groups (in Account resources)](https://cloud.ibm.com/account/resource-groups){: external} and hit Create.

It might appear more convenient to put all service instances of a certain resource group in scope of an access group.
However, note that a service instance can only belong to one resource group and the assignment of instances into resource groups cannot be changed anymore.
This also means that the resource group assignment can only happen at service instance creation.
Therefore, resource groups may not provide enough flexibility if assignments of service instances to resource groups might need to change.

Only reason for using resource groups is a clear separation of service instances, and access groups will be simpler.
Also, if additional service instances are created in a resource group, all users having access to the resource group will see them automatically without updating access groups.  

## Implementation
{: #implementation-org}
{: step}

The following steps are needed to create the governance structure to achieve the objectives outline above.
Note that these steps are generic to IBM Cloud and not specific to Qiskit Runtime, with the only exception being the custom role details which employs Qiskit Runtime specific actions.

As an example, this tutorial uses two projects, one called `ml` and the other one called `finance`.

### IAM Settings
{: #iam-org}
{: step}

There are some options to configure the IAM (Identity and Access Management) settings of administrator's account.
To review and configure, go to [Manage -- IAM -- Settings](https://cloud.ibm.com/iam/settings){: external}.
The "User list visibility" setting determines, whether users can see each other (irrespective of project assignment). Select "Enabled" to restrict visibility of users, meaning users in your account cannot see each other (note: this is irrespective on whether they have access to the same resources, i.e. are in the same project). Review [link](https://cloud.ibm.com/docs/account?topic=account-iam-user-setting){: external} for more details.
API keys are needed by users as part of this tutorial approach, this setting should be set to disabled (or alternatively specific permissions should be given to users).

![IAM settings](images/org-guide-iam-settings.png "User list visibility"){: caption="Figure 1. IAM settings page with User list visibility enabled" caption-side="bottom"}

### Create Qiskit Runtime Service Instances
{: #create-instance-org}
{: step}

Unless you have created Qiskit Runtime service instances already, you can do so now.
(If you decide to [use resource groups](##resource-groups) instead of selecting individual service instances, make sure to create the service instances on the appropriate resource group).

The name of the service instance (e.g. QR-ml) will be used later in a reference from the access group.

### Create Access Groups for Projects
{: #create-group-org}
{: step}

We use access groups as a simple yet powerful means to consistently assign access to users.
First we create a access group for each project.
Each access group will get the minimal set of permissions required to work with project resources.
We can then map users to access groups to complete the mapping.

Each access group will a custom role that allows users to perform actions to work with Qiskit Runtime service instances.

Let us create the custom role. In [Manage -- IAM -- Roles](https://cloud.ibm.com/iam/roles){: external}, click on Create.
Enter name, ID, description and select Qiskit Runtime from the service as shown in the image.

![This image shows a custom role being created.](images/org-guide-create-custom-role.png "Using the Configure your resource panel to create a custom role"){: caption="Figure 4. Creating a custom role" caption-side="bottom"}

Select the following roles:

* quantum-computing.device.read
* quantum-computing.job.cancel
* quantum-computing.job.create
* quantum-computing.job.read
* quantum-computing.program.create
* quantum-computing.program.delete
* quantum-computing.program.read
* quantum-computing.program.update
* quantum-computing.user.logout

If you would like to allow users to delete jobs, add quantum-computing.job.delete as well.

![Defining actions for the custom role](images/org-guide-custom-role-actions.png "Defining actions for the custom role"){: caption="Figure 5. Define actions for the custom role" caption-side="bottom"}


Hit Create.

In [Manage -- IAM -- Access groups](https://cloud.ibm.com/iam/groups){: external}, click Create, enter a name like project-ml and a description.
Select the Access tab and click on Assign access.

In the Service list, search for Qiskit Runtime and select it.

![Select Service for Access Group](images/org-guide-create-access-group-1.png "Select Service for Access Group"){: caption="Figure 6. Select Service for Access Group" caption-side="bottom"}


Hit Next.
In Resources, select Specific resources, and for Attribute type, use Service Instance.
From the drop down list, select the service instance you want to add to the access group (in the example: QR-ml).
(If you decide to [use resource groups](##resource-groups) instead of selecting individual service instances, select the resource group here)

![Select Resources for Access Group](images/org-guide-create-access-group-2.png "Select Resources for Access Group"){: caption="Figure 7. Select Resources for Access Group" caption-side="bottom"}


Hit Next.
For Roles and actions, select Viewer as well as the custome role created previously.

![Select Roles and actions for Access Group](images/org-guide-create-access-group-3.png "Select Roles and actions for Access Group"){: caption="Figure 8. Select Roles and actions for Access Group" caption-side="bottom"}


Click on Add, then Assign.

If you want to give an access group permissions to several service instances, repeat this sequence to add a policy for every service instance.

## Steps For an Example Scenario
{: #steps-org}
{: step}

In our example, we want to create the following setup:
* we have two projects, `ml` and `finance`
  * the ml project should have access to the service instances `QR-ml` and `QR-common`
  * the finance project should have access to the service instances `QR-finance` and `QR-common`
* we have three users
  * user Fatima should have access to the ml project
  * user Ravi should have access to the finance project
  * user Amyra should have access to both projects
* users are defined in an App ID instance and assignments to projects is also done there
* users should be able to delete jobs

The steps to implement this setup are:
* the Cloud administrator creates an App ID instance and ensure it is linked in the Cloud administrator's account
  * the ID provider URL is passed on to users
* the Cloud administrator creates service instances QR-ml, QR finance and QR-common
* the Cloud administrator creates a custom rule including the quantum-computing.job.delete action
* the Cloud administrator creates access groups
  * one access group for ml having access to QR-ml and QR-common. This access group should get a dynamic rule agains the App ID IDP, to test whether `project` contains `ml`
  * an access group for finance, having access to QR-finance and QR-common. This access group should get a dynamic rule agains the App ID IDP, to test whether `project` contains `finance`
* the IDP administrator uses the App ID instance that the Cloud administrator created and defines the three users
  * for Fatima, the custom attributes should contain `{"project":"ml"}`
  * for Ravi, the custom attributes should contain `{"project":"finance"}`
  * for Amyra, the custom attributes should contain `{"project":"ml finance"}`
* users can log in through the ID provider URL, create API keys and work with their projects' service instances

## Additional Considerations
{: #considerations-org}
{: step}

### Auditability
{: #audits-org}


Activity tracker logs significant actions performed on Qiskit Runtime service instances; create an instance of Activity Tracker in the region of your Qiskit Runtime instances to get an audit trail of events.
Refer to the [Activity Tracker documentation page of Qiskit Runtime](https://cloud.ibm.com/docs/quantum-computing?topic=quantum-computing-at_events){: external} for details on the events logged.
This audit log contains the fields `initiator_authnName` and `initiator_authnId` that match the name shown in [Manage -- Access (IAM) -- Users](https://cloud.ibm.com/iam/users){: external}, resp. when clickin on the user name and then Details in the IAM ID field on the right hand side.

![Example of an Activity Tracker event](imagesorg-guide-audit-example.png "Example of an Activity Tracker event"){: caption="Figure 12. Example of an Activity Tracker event" caption-side="bottom"}


To capture App ID events, open your App ID instance, open Manage Authentication -- Authentication settings and enable Runtime Activity.

### More Fine Grained Roles
{: #more-roles-org}

The actions in the custom roles can be used for more fine grained access control.
For instance, some users might get full access to work on service instances while others might only be granted read access to service instances, programs and jobs.
For that, define two different custom roles (e.g. remove all cancel, delete and update roles from the reader custom role, and include all actions in the custom role for the writer).
Then add them to two different access groups accordingly.

When using dynamic rules (i.e. the IDP administrator manages access through custom IDP user attributes), make sure to not use IDP custom user attributes that are substrings of each other:
For instance, don't use `ml` and `ml-reader`, as the string comparison of ml would also hit ml-reader.
Instead, use `ml-reader` and `ml-writer` to avoid this conflict.

### Other Cloud Resources
{: #other-cloud-rsc-org}

The pattern used in this tutorial can be used to manage access to other Cloud resources as well.
Include the appropriate permissions to the access groups of the relevant projetcs.

### Nested Project Structures
{: #nest-org}

In this tutorial, the mapping of users to projects and service instances was kept simple.
Through associating several users with access groups and referencing service instances from several access groups, any arbitrary mapping can be implemented.
This can accomodate a hierarchical structure, e.g. align to how users might be assigned to the Hub/Group/Project access structure in the IBM Quantum Platform;
e.g., a group could be an access group that is assigned to all service instances of the group's projects.
Users who should get access to all group's projects would then only have to be added to the group's access group.

### Consistent and Repeatable Deployment of Configuration
{: #repeat-org}

The steps of this tutorial can be automated for a consistent and repeatable management of users, projects, and the mapping between those.
Please refer to the [Terraform IBM Cloud Provider documentation (external link)](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external} for more details on template contents.

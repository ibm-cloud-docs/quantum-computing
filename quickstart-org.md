---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-26"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Getting started for organizations
{: #quickstart-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

When working in an organization where several individuals might work on several projects, the governance of consuming Qiskit Runtime can become a topic.
Access management can be used to enable collaboration of users working on the same project, as well as to restrict visibility of users and projects that should be isolated from each other.
Managing access becomes particularly relevant when paid Qiskit Runtime resources are shared.

IBM Cloud provides various ways to implement such mechanism -- this tutorial is one way to achieve these objectives, but may not be the only way.
{: shortdesc}


## Involved Personas
{: #personas}
{: step}

The are several main personas used in this tutorial:

* **User**: a user who gets access to Qiskit Runtime resources (service instances) and can potentially collaborate with other users on these resources. Users will only be able to access instances they have been given visibility to. Also, they will not be able to create or delete resources.
* **Cloud administrator**: an owner of an IBM Cloud account owning Qiskit Runtime resources and manages access of other users to these resources. As resource owner, the administrator will also be charged for paid resources.
* **IDP administrator**: an administrator who defines identities and their attributes in an identity provider (IDP).

Other terms used in this tutorial are:

* **Project**: a grouping unit that is used to enable users to work on the same resources. In that case, users are seen as part of the same project. 

The project is not related to the "project" notion of the IBM Quantum Platform. See [Nested project structures](#nest-org) for more information on this topic.
{: note}

## Planning
{: #planning}
{: step}

The planning process includes decisions on the following aspects:

1. decide on how where user identities are defined: are users IBM Cloud users or are users identities in another ID provider?
  * In the latter case, this tutorial will show how to create an ID provider in IBM Cloud
2. decide on who manages resource assignments: does the Cloud administrator or the IDP administrator perform assignment of users to project resources?
3. what are the projects and which service instances should belong to these?
4. which users should get visibility to which projetcs?
5. should users be able to delete jobs?
  * Keeping jobs in service instances gives more traceability to how billing cost was induced.

## Implementation
{: #implementation}
{: step}

The following steps are needed to create the governance structure to achieve the objectives outline above.

As an example, this tutorial uses two projects, one called `ml` and the other one called `finance`.

### Configure IAM
{: #iam-settings}
{: step}

There are some options to configure the IAM (Identity and Access Management) settings of administrator's account.
To review and configure, go to [Manage -- IAM -- Settings](https://cloud.ibm.com/iam/settings){: external}.
The "User list visibility" setting determines, whether users can see each other (irrespective of project assignment). Select "Enabled" to restrict visibility of users. Review [Controlling user visibility](https://cloud.ibm.com/docs/account?topic=account-iam-user-setting){: external} for more details.
API keys are needed by users as part of this tutorial approach, this setting should be set to disabled (or alternatively specific permissions should be given to users).

![IAM settings](images/org-guide-iam-settings.png "User list visibility"){: caption="Figure 1. IAM settings page with User list visibility enabled" caption-side="bottom"}

### Optional: Using App ID as ID Provider
{: #app-id}
{: step}

This step is only relevant if you want to enable users from an ID provider; if your users have IBM Cloud accounts, this is not needed.
App ID creates an ID provider which allows to add users directly in App ID, as well as connecting to other external ID providers.

To start, [create an App ID instance from the IBM Cloud catalog](https://cloud.ibm.com/catalog/services/app-id){: external}.
The location is not critical, but it is best practice to keep it in the same location as the Qiskit Runtime service, i.e. Washington DC (us-east).
The lite plan is free of charge and enough to get started.
If needed, it is possible to upgrade seamlessly to the graduated tier of your App ID instance after starting with the lite tier.
The graduated tier is paid per event and user as going above the lite tier limits.
The graduated tier also supports additional features such as multi-factor authentication.

So create the App ID instance, give it a name.
Other settings like [resource groups](#resource-groups) can optionally be modified.

![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 2. Create your APP ID instance" caption-side="bottom"}

Read and agree to the terms and hit Create.

In the IBM Cloud [resource list](https://cloud.ibm.com/resources){: external}, find your App ID instance and view its details.
We use the Cloud Directory capability to add users to the ID provider.
Refer to the [App ID documentation](https://cloud.ibm.com/docs/appid){: external} for instructions how to integrate other ID providers into App ID.

Open Manage Authentication and deselect the Facebook and Google login options if you do not need them.
Open Manage Authentication -- Cloud Directory -- Settings to adjust whether user logins should be based on username or password.
Open Manage Authentication -- Cloud Directory -- Password Policies to define the password strength if desired.

You can customize the appearance of the login page through the Login Customization page on the navigation bar.

As the next step, we integrated the App ID instance as ID provider to the administrator's account.
To do this, go to [Manage -- Acces (IAM) -- Identity Providers](https://cloud.ibm.com/iam/identity-providers){: external}.
Type should be set to IBM Cloud App ID, then hit Create.

Specify a name and select the App ID instance from the drop down list.
Select the checkbox to enable the ID provider.

![Create identity provider](images/org-guide-idp-reference.png "Create identity provider"){: caption="Figure 3. Create identity provider page" caption-side="bottom"}

After hitting Create, the Default IdP URL shown will be the UDL that users can use to log in.
The administrator needs to pass this URL to users.

### Create Qiskit Runtime service instances
{: #org-runtime}
{: step}

Unless you have created Qiskit Runtime service instances already, you can do so now.
(If you decide to [use resource groups](#rsc-grp-org) instead of selecting individual service instances, make sure to create the service instances on the appropriate resource group).

The name of the service instance (e.g. QR-ml) will be used later in a reference from the access group.

### Create Access Groups for projects
{: #access-groups-org}
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

![Defining actions for the custom role](./org-guide-custom-role-actions.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 5. Create your APP ID instance" caption-side="bottom"}

Hit Create.

In [Manage -- IAM -- Access groups](https://cloud.ibm.com/iam/groups){: external}, click Create, enter a name like project-ml and a description.
Select the Access tab and click on Assign access.

In the Service list, search for Qiskit Runtime and select it.

![Select Service for Access Group](./org-guide-create-access-group-1.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 6. Create your APP ID instance" caption-side="bottom"}

Hit Next.
In Resources, select Specific resources, and for Attribute type, use Service Instance.
From the drop down list, select the service instance you want to add to the access group (in the example: QR-ml).
(If you decide to [use resource groups](#rsc-grp-org) instead of selecting individual service instances, select the resource group here)

![Select Resources for Access Group](./org-guide-create-access-group-2.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 7. Create your APP ID instance" caption-side="bottom"}

Hit Next.
For Roles and actions, select Viewer as well as the custome role created previously.

![Select Roles and actions for Access Group](./org-guide-create-access-group-3.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 8. Create your APP ID instance" caption-side="bottom"}

Click on Add, then Assign.

If you want to give an access group permissions to several service instances, repeat this sequence to add a policy for every service instance.

### Managing Access through the IDP Administrator: Add Dynamic Rule to Access Group
{: #manage-access-org}
{: step}

If IDP user attributes are used to manage access, the access group needs a dynamic rule to test whether the access group should be applied to an ID provider user logging in.

The dynamic rules are evaluated during login, so changes in the ID provider user attributes are picked up on the next login of the user.
{: note}

On the access group, click on Dynamic rules, then Add.
Provide a name, select Users federated by IBM Cloud AppID as Authentication method, and choose the ID provider from the Identity Provider drop down list.

![Create Dynamic Rule](./org-guide-create-dynamic-rule1.png)

Click on Add a condition.
In the Allow users when field, enter the attribute key used in ID provider user attributes, such as `project`.
Select Contains as the Qualifier.
In Values, enter the value, such as `ml`

![Add Condition to Dynamic Rule](./org-guide-create-dynamic-rule2.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 9. Create your APP ID instance" caption-side="bottom"}

Click on Add.

Please note the qualifier is `Contains`: if the values of different projects are substrings of other projects, e.g. when using `ml` as well as using `chemlab`, the contains qualifier using ml will trigger on both values.
A strategy to reduce such unintended matches is to prefix and/or suffix values.
However, this substring match behaviour can be useful when a user attribute contains several values, i.e. should cause visibility to several projetcs.

### Using Cloud Users: Invite Users
{: #cloud-users-org}
{: step}

When users use their own IBM Cloud accounts to access project resources, users can be invited.
Go to [Manage -- Access (IAM)](https://cloud.ibm.com/iam/overview){: external} and click on [Invite users](https://cloud.ibm.com/iam/users/invite_users){: external}.
Enter the email addresses of users to be added.
Select the access group or groups of the projects that the additional users should be part of.
This assignment can be changed or extended later on.

### ID Provider Users: Add Users
{: #provider-users-org}
{: step}

When using App ID as ID provider with the Cloud directory, you can create users in the Cloud UI.
Open the App ID instance page, e.g. from the [resource list](https://cloud.ibm.com/resources){: external}.
Select Manage Authentication -- Cloud Directory -- Users, and hit Create User.
Enter the user details to add users.

### Managing Access through the IDP Administrator: Create or Modify Users' Assignments to Projects
{: #idp-admin-org}
{: step}

If the setup is to allow IDP administrator to define user/project assignments, you can define project values in the user's attributes.
Open the App ID instance page, e.g. from the [resource list](https://cloud.ibm.com/resources){: external}.
Select Manage Authentication -- Cloud Directory -- Users, and open the user by clicking on it.
Scroll down to the Custom Attributes, and click on Edit.
Enter a key value pair that can will checked by the dynamic rules of the access groups.
In our example, add
```
{"project":"ml"}
```
and click Save.

You can add several values in the same string (e.g. stating `{"project":"ml finance"}`); the contains qualifier of the dynamic rule will detect a match of a substring.
Please note that if the values of different projects are substrings of other projects, e.g. when using `ml` as well as using `chemlab`, the contains qualifier using ml will trigger on both values.
A strategy to reduce such unintended matches is to prefix and/or suffix values.
However, this substring match behaviour can be useful when a user attribute contains several values, i.e. should cause visibility to several projetcs.

Note: The check of access groups against this attribute is done on every login, so changes in the ID provider user attributes will be effective on the next login of the user.

### Managing Access through the Cloud Administrator: Modify Users' Assignments to Projects
{: #cloud-admin-access-org}
{: step}

If ID provider users are used as identities, note that IBM Cloud knows about ID provider users only after the first login.
At that point, users will not have any permissions.
Accordingly, access can only be given or modified after the IDP user has logged in for the first time.

To change access of users and add them to projects, refer to the Changing Access of Users section.

Go to [Manage -- Access (IAM) -- Users](https://cloud.ibm.com/iam/users){: external} and click on the user.

![Change User Access](./org-guide-manage-user.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 10. Create your APP ID instance" caption-side="bottom"}

Add additional access groups with Assign group, or remove the user from the access group with the three dots on the right hand side (Remove user)

## User Flow
{: #user-flow-org}
{: step}

Users having an IBM Cloud account can login through the [IBM Cloud portal](https://cloud.ibm.com/){: external}.

ID provider users login through the ID provider URL that has been presented previously to the administrator setting up the ID provider to the IBM Cloud account.
The administrator can always go to [Manage -- Access (IAM) -- Identity providers](https://cloud.ibm.com/iam/identity-providers){: external} to look up the ID provider URL.

To work with Qiskit Runtime, users can create an API key ([Manage -- Access (IAM) -- API keys](https://cloud.ibm.com/iam/apikeys){: external}) and use it for service instances they have got access to.

## Steps For an Example Scenario
{: #user-steps-org}
{: step}

In our example, we want to create the following setup:
* we have two projects, `ml` and `finance`
  * the ml project should have access to the service instances `QR-ml` and `QR-common`
  * the finance project should have access to the service instances `QR-finance` and `QR-common`
* we have three users
  * user Anna should have access to the ml project
  * user Barry should have access to the finance project
  * user Chris should have access to both projects
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
  * for Anna, the custom attributes should contain `{"project":"ml"}`
  * for Barry, the custom attributes should contain `{"project":"finance"}`
  * for Chris, the custom attributes should contain `{"project":"ml finance"}`
* users can log in through the ID provider URL, create API keys and work with their projects' service instances

## Additional Considerations
{: #consider}
{: step}

### Auditability
{: #auditability-org}
{: step}

Activity tracker logs significant actions performed on Qiskit Runtime service instances; create an instance of Activity Tracker in the region of your Qiskit Runtime instances to get an audit trail of events.
Refer to the [Activity Tracker documentation page of Qiskit Runtime](https://cloud.ibm.com/docs/quantum-computing?topic=quantum-computing-at_events){: external} for details on the events logged.
This audit log contains the fields `initiator_authnName` and `initiator_authnId` that match the name shown in [Manage -- Access (IAM) -- Users](https://cloud.ibm.com/iam/users){: external}, resp. when clickin on the user name and then Details in the IAM ID field on the right hand side.

![Example of an Activity Tracker event](./org-guide-audit-example.png)
![Create App ID instance](images/org-guide-create-appid.png "Create App ID instance"){: caption="Figure 11. Create your APP ID instance" caption-side="bottom"}

### Resource Groups
{: #rsc-grp-org}
{: step}

Instead of access groups directly referencing Qiskit Runtime service instances, service instances can be organized in resource groups.
Service instances can only be created in existing resource groups, and access groups can also only refer to existing resource groups -- therefore resource groups need to be created first.
To do so, go to [Manage -- Account -- Resource groups (in Account resources)](https://cloud.ibm.com/account/resource-groups){: external} and hit Create.

It might appear more convenient to put all service instances of a certain resource group in scope of an access group.
However, note that a service instance can only belong to one resource group and the assignment of instances into resource groups cannot be changed anymore.
This also means that the resource group assignment can only happen at service instance creation.
Therefore, resource groups may not provide enough flexibility if assignments of service instances to resource groups might need to change.

### More Fine Grained Roles
{: #fine-grain-org}
{: step}

The actions in the custom roles can be used for more fine grained access control.
For instance, some users might get full access to work on service instances while others might only be granted read access to service instances, programs and jobs.
For that, define two different custom roles (e.g. remove all cancel, delete and update roles from the reader custom role, and include all actions in the custom role for the writer).
Then add them to two different access groups accordingly.

When using dynamic rules (i.e. the IDP administrator manages access through custom IDP user attributes), make sure to not use IDP custom user attributes that are substrings of each other:
For instance, don't use `ml` and `ml-reader`, as the string comparison of ml would also hit ml-reader.
Instead, use `ml-reader` and `ml-writer` to avoid this conflict.

### Other Cloud Resources
{: #other-rsc-org}
{: step}

The pattern used in this tutorial can be used to manage access to other Cloud resources as well.
Include the appropriate permissions to the access groups of the relevant projetcs.

### Nested Project Structures
{: #nest-org}
{: step}

In this tutorial, the mapping of users to projects and service instances was kept simple.
Through associating several users with access groups and referencing service instances from several access groups, any arbitrary mapping can be implemented.
This can accomodate a hierarchical structure, e.g. align to how users might be assigned to the Hub/Group/Project access structure in the IBM Quantum Platform;
e.g., a group could be an access group that is assigned to all service instances of the group's projects.
Users who should get access to all group's projects would then only have to be added to the group's access group.

### Consistent and Repeatable Deployment of Configuration
{: #repeatable-org}
{: step}

The steps of this tutorial can be automated for a consistent and repeatable management of users, projects, and the mapping between those.
Please refer to the [Terraform IBM Cloud Provider documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external} for more details on template contents.

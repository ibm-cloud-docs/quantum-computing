---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-26"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization, appid

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Use App ID as the ID provider
{: #appid-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This step is only relevant if you want to enable users from an ID provider; if your users have IBM Cloud accounts, this is not needed.
App ID creates an ID provider which allows to add users directly in App ID, as well as connecting to other external ID providers.

To start, [create an App ID instance from the IBM Cloud catalog](https://cloud.ibm.com/catalog/services/app-id){: external}.
The location is not critical, but it is best practice to keep it in the same location as the Qiskit Runtime service, i.e. Washington DC (us-east).
The lite plan is free of charge and enough to get started.
If needed, it is possible to upgrade seamlessly to the graduated tier of your App ID instance after starting with the lite tier.
The graduated tier is paid per event and per user as going above the lite tier limits of these metrics.
The graduated tier also supports additional features such as multi-factor authentication.
The Cloud administrator as the owning account of the App ID instance will be charged for any charges of graduated tier instances.

So create the App ID instance, give it a name.
Other settings like [resource groups](##resource-groups) can optionally be modified.

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

## Managing Access through the IDP Administrator: Add Dynamic Rule to Access Group
{: #manage-idp-org}
{: step}

If IDP user attributes are used to manage access, the access group needs a dynamic rule to test whether the access group should be applied to an ID provider user logging in.
Note: the dynamic rules are evaluated during login, so changes in the ID provider user attributes are picked up on the next login of the user.

On the access group, click on Dynamic rules, then Add.
Provide a name, select Users federated by IBM Cloud AppID as Authentication method, and choose the ID provider from the Identity Provider drop down list.

![Create Dynamic Rule](images/org-guide-create-dynamic-rule1.png "Create Dynamic Rule"){: caption="Figure 9. Create Dynamic Rule" caption-side="bottom"}


Click on Add a condition.
In the Allow users when field, enter the attribute key used by the IDP administrator in ID provider user attributes, such as `project` (this string is a convention defined in the planning section).
Select Contains as the Qualifier.
In Values, enter the value, such as `ml`.
This is the same value that is the IDP administator uses in the ID provider user, and for all practical purposes should be the project name.

![Add Condition to Dynamic Rule](images/org-guide-create-dynamic-rule2.png "Add Condition to Dynamic Rule"){: caption="Figure 10. Add Condition to Dynamic Rule" caption-side="bottom"}


You might want to increase the Session duration to increase the period before users have to log back in.
Logged-in users keep their access group membership for that period, and re-evaluation takes only place on the next log in.
Click on Add.

Please note the qualifier is `Contains`: if the values of different projects are substrings of other projects, e.g. when using `ml` as well as using `chemlab`, the contains qualifier using ml will trigger on both values.
A strategy to reduce such unintended matches is to prefix and/or suffix values.
However, this substring match behaviour can be useful when a user attribute contains several values, i.e. should cause visibility to several projetcs.

## ID Provider Users: Add Users
{: #add-user-org}
{: step}

When using App ID as ID provider with the Cloud directory, you can create users in the Cloud UI.
Open the App ID instance page, e.g. from the [resource list](https://cloud.ibm.com/resources){: external}.
Select Manage Authentication -- Cloud Directory -- Users, and hit Create User.
Enter the user details to add users.

## Managing Access through the IDP Administrator: Create or Modify Users' Assignments to Projects
{: #create-assigment-org}
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

Note that `project` corresponds to the convention defined in the planning section.
`ml` is be the project name to be used for that user.

You can add several values in the same string (e.g. stating `{"project":"ml finance"}`); the contains qualifier of the dynamic rule will detect a match of a substring.
Please note that if the values of different projects are substrings of other projects, e.g. when using `ml` as well as using `chemlab`, the contains qualifier using ml will trigger on both values.
A strategy to reduce such unintended matches is to prefix and/or suffix values (e.g. use `-ml-` and `-chemlab-`).
However, this substring match behaviour can be useful when a user attribute contains several values, i.e. should cause visibility to several projetcs.

Note: The check of access groups against this attribute is done on every login, so changes in the ID provider user attributes will be effective on the next login of the user.

## User Flow
{: #user-org}
{: step}

ID provider users login through the ID provider URL that has been presented previously to the administrator setting up the ID provider to the IBM Cloud account.
The administrator can always go to [Manage -- Access (IAM) -- Identity providers](https://cloud.ibm.com/iam/identity-providers){: external} to look up the ID provider URL.

To work with Qiskit Runtime, users can create an API key ([Manage -- Access (IAM) -- API keys](https://cloud.ibm.com/iam/apikeys){: external}) and use it for service instances they have got access to.

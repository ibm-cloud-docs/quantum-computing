---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-22"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Use Cloud as the ID provider
{: #cloud-provider-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial describes how to work with users and gives instructions for users to access the environment.  This topic is only relevant if you want to enable users who have IBM Cloud accounts. However, you can use IBM Cloud in conjunction with other ID providers if you choose. Follow the instructions in [this topic](/docs/quantum-computing?topic=appid-org) if you want to set up a different ID provider.
{: shortdesc}

## Invite users
{: #invite-cloud-org}
{: step}

1. Ensure that the users you want to invite have IBM Cloud accounts.
2. Go to Manage → Access (IAM) and click [Invite users](https://cloud.ibm.com/iam/users/invite_users){: external}.
3. Enter the email addresses of users to be added.
4. Select the access group or groups of the projects that the users should be part of. These assignments can be changed later.

## Modify users' project assignments
{: #cloud-assign-user-org}
{: step}

This step applies to Cloud users that have been invited as well as ID provider users which have been added to the account and are managed through the Cloud administrator. Both types of users can be managed by following these steps.

If ID provider users are included, IBM Cloud does not "know about" them until after their first login. At that point, users will not have any permissions.  Thus, access can only be given or modified after the IDP user has logged in for the first time.
{: note}

1. Go to [Manage → Access (IAM) → Users](https://cloud.ibm.com/iam/users){: external} and click on the user.
   ![Change User Access](images/org-guide-manage-user.png "Change User Access"){: caption="Figure 11. Change User Access" caption-side="bottom"}
2. Add access groups with **Assign group**, or remove the user from an access group by clicking the three dot menu and choosing **Remove user**.

## User Flow
{: #user-org}
{: step}

1. Users can log in through the [IBM Cloud portal](https://cloud.ibm.com/){: external}.
2. To work with Qiskit Runtime, users will create an API key by going to ([Manage → Access (IAM) → API keys](https://cloud.ibm.com/iam/apikeys){: external}).  They will use it for service instances they can access.
3. For further information, users can review the Qiskit Runtime documentation, starting with [Getting started, Step 2](/docs/quantum-computing?topic=quickstart#install-packages).

## Next steps
{: #next-steps-org}

- See [steps for an example scenario](/docs/quantum-computing?topic=quickstart-org#steps-org) for an end-to-end example.
- See [additional considerations](/docs/quantum-computing?topic=quickstart-org#steps-org#considerations-org) for more information.

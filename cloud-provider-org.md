---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-26"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 15m
---

{{site.data.keyword.attribute-definition-list}}

# Manage IBM Cloud users
{: #cloud-provider-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial how to use IBM Cloud to enable users who have IBM Cloud accounts and gives instructions for users to access the environment.
{: shortdesc}

To manage ID provider users instead, follow the instructions in one of these topics:

* [Manage ID provider users with the ID provider](/docs/quantum-computing?topic=quantum-computing-appid-org)
* [Manage ID provider users with IBM Cloud](/docs/quantum-computing?topic=quantum-computing-appid-cloud-org)

## Invite users
{: #invite-cloud-org}
{: step}

1. Ensure that the users that you want to invite have IBM Cloud accounts.
2. Go to Manage → Access (IAM) and click [Invite users](https://cloud.ibm.com/iam/users/invite_users){: external}.
3. Enter the email addresses of users to be added.
4. Select the access group or groups of the projects that the users will be part of. These assignments can be changed later.
5. Click Add to confirm the access group selection.
6. Click Invite to send the invitation to the users.

   Users cannot be managed until they accept the invitation and log in at least once.
   {: note}

## Optional: Modify users' project assignments
{: #cloud-assign-user-org}
{: step}

1. Go to [Manage → Access (IAM) → Users](https://cloud.ibm.com/iam/users){: external} and click the user.
   ![Change User Access](images/org-guide-manage-user.png "Change User Access"){: caption="Figure 11. Change User Access" caption-side="bottom"}
2. Add access groups with **Assign group** or remove the user from an access group by clicking the three dot menu and choosing **Remove user**.

## User Flow
{: #user-org}
{: step}

1. After they accept an invitation, users can log in through the [IBM Cloud portal](https://cloud.ibm.com/){: external}.
2. To work with Qiskit Runtime service instances, users must create an API key by going to ([Manage → Access (IAM) → API keys](https://cloud.ibm.com/iam/apikeys){: external}).
3. For further information, users can review [Getting started, Step 2](/docs/quantum-computing?topic=quantum-computing-quickstart#install-packages).

## Example scenario
{: #steps-cloud-org}

In our example, we want to create the following setup:

* We have two projects, `ml` and `finance`.
   * The `ml` project should have access to the service instances `QR-ml` and `QR-common`.
   * The `finance` project should have access to the service instances `QR-finance` and `QR-common`.
* We have three users:
   * Fatima should have access to the `ml` project.
   * Ravi should have access to the `finance` project.
   * Amyra should have access to both projects.
* We will use access groups without resource groups.
* Users are defined in IBM Cloud and project assignments are done there as well.
* Users should be able to delete jobs.

The steps to implement this setup are:

2. The Cloud administrator creates three service instances: `QR-ml`, `QR finance`, and `QR-common`.
3. The Cloud administrator creates a custom rule that includes the `quantum-computing.job.delete` action.
1. The Cloud administrator creates two access groups:
   * The `ml` access group can access `QR-ml` and `QR-common`.
   * The `finance` access group can access `QR-finance` and `QR-common`.
1. The Cloud administrator invites cloud users to the appropriate project. Specifically, they invite and assign users to an access group that includes the project.
   * Fatima is added to the "ml" access group.
   * Ravi is added to the "finance" access group.
   * Amyra is added to both the "ml" and "finance" access groups.
1. Users can log in through the IBM Cloud portal, create API keys, and work with their projects' service instances.

## Next steps
{: #next-steps-org}

* See [Additional considerations](/docs/quantum-computing?topic=quantum-computing-considerations-org) for more information.

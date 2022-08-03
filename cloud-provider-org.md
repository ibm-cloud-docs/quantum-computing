---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-03"

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

* [Manage ID provider users via the ID provider](/docs/quantum-computing?topic=quantum-computing-appid-org)
* [Manage ID provider users via IBM Cloud](/docs/quantum-computing?topic=quantum-computing-appid-cloud-org)

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

1. Go to [Manage → Access (IAM) → Users](https://cloud.ibm.com/iam/users){: external} and click on the user.
   ![Change User Access](images/org-guide-manage-user.png "Change User Access"){: caption="Figure 11. Change User Access" caption-side="bottom"}
2. Add access groups with **Assign group** or remove the user from an access group by clicking the three dot menu and choosing **Remove user**.

## User Flow
{: #user-org}
{: step}

1. Users can log in through the [IBM Cloud portal](https://cloud.ibm.com/){: external}.
2. To work with Qiskit Runtime, users will create an API key by going to ([Manage → Access (IAM) → API keys](https://cloud.ibm.com/iam/apikeys){: external}).  They will use it for service instances they can access.
3. For further information, users can review [Getting started, Step 2](/docs/quantum-computing?topic=quantum-computing-quickstart#install-packages).

## Next steps
{: #next-steps-org}

* See [additional considerations](/docs/quantum-computing?topic=quantum-computing-considerations-org) for more information.

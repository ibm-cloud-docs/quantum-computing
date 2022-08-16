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

# Configure Qiskit Runtime for an organization
{: #quickstart-steps-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

Follow these steps to start setting up Qiskit runtime.
{: shortdesc}

## Configure IAM settings
{: #iam-org}
{: step}

First, there are some settings to configure in the administrator's Identity and Access Management (IAM) account. To review and configure these settings, go to [Manage → IAM → Settings](https://cloud.ibm.com/iam/settings){: external}.

* **User list visibility** determines whether users can see each other, regardless of project assignment. The `enabled` setting restricts user visibility.  That is, users in your account cannot see each other, even if they can access the same resources. Choose the appropriate value for your environment. See [Controlling user visibility](https://cloud.ibm.com/docs/account?topic=account-iam-user-setting){: external} for more information.
* **API key creation** controls whether users can create API keys.  In Qiskit Runtime, it is common to use API keys. If API keys are being used, choose `disabled`.  Alternatively, you can give specific permissions to each user.

![IAM settings](images/org-guide-iam-settings.png "User list visibility"){: caption="Figure 1. IAM settings page with User list visibility enabled" caption-side="bottom"}

## (Optional) Create resource groups
{: #crt-rsc-grp-org}
{: step}

Skip this step if you are using access groups that directly reference Qiskit Runtime service instances.

If you chose to use resource groups, go to [Manage → Account → Resource groups (in Account resources)](https://cloud.ibm.com/account/resource-groups){: external} and click **Create**.

## Create Qiskit Runtime service instances
{: #create-instance-org}
{: step}

If you have already created Qiskit Runtime service instances, skip this step.

* If you are using resource groups, make sure to create the service instances on the appropriate resource group.
* The service instance name, such as `QR-ml`, is needed for access group references.

1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, select the Create tab, then choose the appropriate service plan, depending on what you need access to:
   * **Lite**: Free simulators-only plan to help you get started with {{site.data.keyword.qiskit_runtime_notm}}. Learn to use {{site.data.keyword.qiskit_runtime_notm}} using our examples and tutorials for one of the pre-built programs available for executing circuits efficiently.
   * **Standard**: A pay-as-you-go model for accessing IBM Quantum systems. Build your own programs and use all the benefits of {{site.data.keyword.qiskit_runtime_notm}} by running on real quantum hardware.
1. After completing the required information, click **Create**.

## Create access groups for projects
{: #create-group-org}
{: step}

First, we create a custom role that allows users to perform actions to work with Qiskit Runtime service instances for each access group.  Next, we create an access group for each project and give that group the minimum set of permissions required to work with the project resources. In a later step, we map users to access groups.

Follow these steps to set up an access group:

1. Create a custom role.
   1. From [Manage → IAM → Roles](https://cloud.ibm.com/iam/roles){: external}, click  `Create`.
   2. Enter a name, ID, description, and select `Qiskit Runtime` from the service, as shown in the image:
      ![This image shows a custom role being created.](images/org-guide-create-custom-role.png "Using the Configure your resource panel to create a custom role"){: caption="Figure 4. Creating a custom role" caption-side="bottom"}
   3. Select the following roles, then click **Create**.
      * quantum-computing.device.read
      * quantum-computing.job.cancel
      * quantum-computing.job.create
      * quantum-computing.job.read
      * quantum-computing.program.create
      * quantum-computing.program.delete
      * quantum-computing.program.read
      * quantum-computing.program.update
      * quantum-computing.user.logout
      * Only select quantum-computing.job.delete if you want to allow users to delete jobs.

      ![Defining actions for the custom role](images/org-guide-custom-role-actions.png "Defining actions for the custom role"){: caption="Figure 5. Define actions for the custom role" caption-side="bottom"}

      You can optionally define more fine grained roles by following [these instructions](/docs/quantum-computing?topic=quantum-computing-considerations-org#more-roles-org).
      {: note}

2. Create an access group.
   1. Navigate to [Manage → IAM → Access groups](https://cloud.ibm.com/iam/groups){: external} and click **Create**.
   2. Enter a name, like `project-ml`, and a description.
3. Assign access to the group.
   1. Select the Access tab and click **Assign access**.
   2. In the Service list, search for **Qiskit Runtime** and select it, then click **Next**.
      ![Select Service for Access Group](images/org-guide-create-access-group-1.png "Select Service for Access Group"){: caption="Figure 6. Select Service for Access Group" caption-side="bottom"}

   3. In Resources, select **Specific resources**. For Attribute type, choose **Service Instance**.
   4. From the drop-down list, select the service instance you want to add to the access group, for example, `QR-ml`. If you are using resource groups, select the resource group instead of selecting individual service instances.  Click **Next**.
      ![Select Resources for Access Group](images/org-guide-create-access-group-2.png "Select Resources for Access Group"){: caption="Figure 7. Select Resources for Access Group" caption-side="bottom"}
   5. For Roles and actions, select **Viewer** and the custom role created previously.  Click **Add**, then **Assign**.
      ![Select Roles and actions for Access Group](images/org-guide-create-access-group-3.png "Select Roles and actions for Access Group"){: caption="Figure 8. Select Roles and actions for Access Group" caption-side="bottom"}
   6. Repeat this step if you want to give an access group permissions to several service instances.

## Set up your ID provider and assign users
{: #setup-idp-org}
{: step}

Follow the steps in the appropriate topic, depending on the ID provider you chose to use:

* [Use IBM Cloud as the ID provider for users that have IBM Cloud accounts](/docs/quantum-computing?topic=quantum-computing-cloud-provider-org)
* [Use IBM Cloud as the ID provider for users that do not have IBM Cloud accounts](/docs/quantum-computing?topic=quantum-computing-appid-cloud-org)
* [Use an ID provider other than IBM Cloud for users that do not have IBM Cloud accounts](/docs/quantum-computing?topic=quantum-computing-appid-org)

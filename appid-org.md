---

copyright:
  years: 2021, 2023
lastupdated: "2022-07-26"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization, appid

subcollection: quantum-computing

content-type: tutorial
completion-time: 15m
---

{{site.data.keyword.attribute-definition-list}}

# Manage ID provider users with the ID provider
{: #appid-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

App ID creates an ID provider so you can add users directly in App ID or connect to other external ID providers. This tutorial describes how to set up your ID provider to work with users that do not have IBM Cloud accounts.
{: shortdesc}

To manage users via IBM Cloud, see one of these topics:

* [Manage IBM Cloud users](/docs/quantum-computing?topic=quantum-computing-cloud-provider-org)
* [Manage ID provider users with IBM Cloud](/docs/quantum-computing?topic=quantum-computing-appid-cloud-org)

{{appid-cloud-org.md#create-appid-cloud}}

## Configure the ID provider
{: #add-users-appid}
{: step}

We will use the **Cloud Directory** capability to add users to the ID provider.
Refer to the [App ID documentation](/docs/appid){: external} for instructions how to integrate other ID providers into App ID.

1. Open the [IBM Cloud resource list](https://cloud.ibm.com/resources){: external}, expand the **Services and software** section, find your App ID instance and click its name to view its details.
2. Click **Manage Authentication** and deselect any login options that you don't need, such as Facebook and Google.
3. Navigate to **Manage Authentication** → **Cloud Directory** → **Settings** and choose whether user logins should use email or usernames.
4. Optional: Open **Manage Authentication** → **Cloud Directory** → **Password Policies** to define the password strength.
5. Optionally open **Login Customization** and customize the appearance of the login page.

## Integrate the App ID instance as the ID provider for the administrator's account
{: #integrate-appid}
{: step}

1. Go to [Manage → Access (IAM) → Identity Providers](https://cloud.ibm.com/iam/identity-providers){: external}. For **Type**, choose **IBM Cloud App ID**, then click **Create**.
2. Specify a name and select the App ID instance from the drop-down list.
3. Select the checkbox to enable the ID provider.
4. The default IdP URL is shown. Share this URL with users when they need to log in.

## Add a dynamic rule to the access group
{: #manage-idp-org}
{: step}

The access group needs a dynamic rule to test whether it should be applied to an IDP user when they log in.

Because the dynamic rules are evaluated during login, any changes are picked up the next time the user logs in.
{: note}

1. Navigate to [Manage → IAM → Access groups](https://cloud.ibm.com/iam/groups){: external} and click your access group to open its details page.
2. Click the **Dynamic rules** tab, then click **Add**.
   * Provide a name.
   * For the Authentication method, choose **Users federated by IBM Cloud AppID**, then select the IDP from the Identity provider drop-down list.
3. Click **Add a condition**, complete the following values, then click **Add**.
   * In the **Allow users when** field, enter the attribute key that is used by the IDP administrator in the ID provider user attributes, such as `project` (this string is a convention that is defined during planning).
   * Select **Contains** as the **Qualifier**.
   * In **Values**, enter the value, such as `ml`. This is the same value that the IDP administrator uses in the IDP user profile definition. It is typically the project name.
   * You might want to increase the **Session duration** to increase the period before users must log back in. Logged-in users keep their access group membership for that period, and reevaluation takes place on the next login.

## Add users
{: #add-user-org}
{: step}

When you use App ID as ID provider with the Cloud directory, you can create users in the Cloud user interface.

1. Open the App ID instance page from the [resource list](https://cloud.ibm.com/resources){: external} Services and software section.
2. Go to **Manage Authentication** → **Cloud Directory** → **Users**, and click **Create User**. Enter the user details.

## Create or modify users' project assignments
{: #create-assigment-org}
{: step}

If the IDP administrator will assign users to projects, you can define project values in the user's attributes.

1. Open the App ID instance page from the [resource list](https://cloud.ibm.com/resources){: external} Services and software section.
2. Go to **Manage Authentication** → **Cloud Directory** → **Users**, and click a user to open it.
3. Scroll down to **Custom Attributes**, and click **Edit**.
4. Enter a key value pair that can will checked by the dynamic rules of the access groups, then click **Save**. You can add several values in the same string (for example, `{"project":"ml finance"}`); the **contains** qualifier of the dynamic rule detects a match of a substring. For our example, add:

   ```text
   {"project":"ml"}
   ```

   The value `project` corresponds to the convention defined in the planning section. `ml` is the project that the user belongs to.

   This check is done on every login, so changes in the ID provider user attributes will be effective when a user next logs in.

## User flow
{: #user-flow-org}

1. A user is sent the ID provider URL for the IBM Cloud account.

   The administrator can always go to [Manage → Access (IAM) → Identity providers](https://cloud.ibm.com/iam/identity-providers){: external} to look up the ID provider URL.
   {: note}

2. To work with Qiskit Runtime service instances, users must create an API key by going to ([Manage → Access (IAM) → API keys](https://cloud.ibm.com/iam/apikeys){: external}).
3. For further information, users can review [Getting started, Step 2](/docs/quantum-computing?topic=quantum-computing-get-started#install-packages).

## Example scenario
{: #steps-appid-org}

In our example, we want to create the following setup:

* We have two projects, `ml` and `finance`.
   * The `ml` project needs access to the service instances `QR-ml` and `QR-common`.
   * The `finance` project needs access to the service instances `QR-finance` and `QR-common`.
* We have three users:
   * Fatima needs access to the `ml` project.
   * Ravi needs access to the `finance` project.
   * Amyra needs access to both projects.
* We will use access groups without resource groups.
* Users are defined in an App ID instance and project assignments are also done there.
* Users should be able to delete jobs.

The steps to implement this setup are:

1. The Cloud administrator creates an App ID instance and ensures that it is linked in the Cloud administrator's account. The administrator notes the ID provider URL to share it with users.
2. The Cloud administrator creates three service instances: `QR-ml`, `QR finance`, and `QR-common`.
3. The Cloud administrator creates a custom rule that includes the `quantum-computing.job.delete` action.
4. The Cloud administrator creates two access groups:
   * The `ml` access group can access `QR-ml` and `QR-common`. This access group needs a dynamic rule for the App ID IDP that accepts users whose `project` attribute contains `ml`.
   * The `finance` access group can access `QR-finance` and `QR-common`. This access group needs a dynamic rule for the App ID IDP that accepts users whose `project` attribute contains `finance`.

5. The IDP administrator uses the App ID instance that the Cloud administrator created and defines the three users:
   * For Fatima, the custom attributes contain `{"project":"ml"}`.
   * For Ravi, the custom attributes contain `{"project":"finance"}`.
   * For Amyra, the custom attributes contain `{"project":"ml finance"}`.
6. Users can log in through the ID provider URL, create API keys, and work with their projects' service instances.

## Next steps
{: #next-steps-appid-org}

For more information, see [additional considerations](/docs/quantum-computing?topic=quantum-computing-considerations-org).

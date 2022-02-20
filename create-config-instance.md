---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Creating and configuring a Quantum Service instance
{: #gettingstarted}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This tutorial walks you through the steps to set up an IBM Quantum service instance and invite users.
{: shortdesc}


1.  [Create an {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} for the organization.
2.  From the [user management page](https://cloud.ibm.com/iam/overview){: external}, invite users to join the account.

   The users must have {{site.data.keyword.cloud_notm}} accounts before they can be invited.
   {: note}

3.  Create a quantum service instance:
      1. From the [{{site.data.keyword.quantum_long_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, select the Create tab, then choose the appropriate service plan, depending on what you need access to:
          - **Lite**: Free simulators-only plan to help you get started with Qiskit Runtime. Learn to use Qiskit Runtime using our examples and tutorials for one of the pre-built programs available for executing circuits efficiently.
          - **Standard**: A pay-as-you-go model for accessing IBM quantum systems. Build your own programs and leverage all the benefits of Qiskit Runtime by running on real quantum hardware.
      2. After completing the required information, click **Create**.
4. From the [{{site.data.keyword.cloud_notm}} console](/iam/overview){: external}, click Manage > Access (IAM) to create an IAM access policy, ideally an access group policy, to give users access to the service instance. Optionally use resource groups, access groups, tags, and so on, to manage resources and access to them.

For more information about roles, including details about program level roles and instructions to work with access groups, see the [IAM access documentation](https://cloud.ibm.com/docs/account?topic=account-userroles){: external}.

## Access roles
{: #access-roles}

Action | Description | Roles
---|---|---
quantum-computing.program.create | Create programs and change program privacy | Manager, Writer
quantum-computing.program.read | View programs and program details | Manager, Reader
quantum-computing.program.delete | Delete programs | Manager, Writer
quantum-computing.program.update | Update programs | Manager, Writer
quantum-computing.job.create | Run jobs | Manager, Writer
quantum-computing.job.read | View job results and logs | Manager, Reader
quantum-computing.job.delete | Delete jobs | Manager, Writer


## Next steps
{: #next-steps}

- Learn how to [access your {{site.data.keyword.quantum_long_notm}} instance](/docs/quantum-computing?topic=quantum-computing-access).
- Learn how to [submit a job](/docs/quantum-computing?topic=quantum-computing-run_job).
- View the [API reference](/apidocs/quantum-computing){: external}.
- Learn about IBM Quantum:
    - [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}
    - [Qiskit](https://qiskit.org/){: external}

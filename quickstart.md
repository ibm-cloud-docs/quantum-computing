---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-07"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Quick start guide
{: #quickstart}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial walks you through the steps to set up a {{site.data.keyword.qiskit_runtime_notm}} service instance, log into your service instance, and run your first job on a quantum computer.
{: shortdesc}


## Create a service instance
{: #create-configure}

1. [Create an {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} for the organization.
2. From the [user management page](https://cloud.ibm.com/iam/overview){: external}, invite users to join the account.

   The users must have {{site.data.keyword.cloud_notm}} accounts before they can be invited.
   {: note}

3. Create a {{site.data.keyword.qiskit_runtime_notm}} service instance:
   1. From the [{{site.data.keyword.quantum_long_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, select the Create tab, then choose the appropriate service plan, depending on what you need access to:
      - **Lite**: Free simulators-only plan to help you get started with {{site.data.keyword.qiskit_runtime_notm}}. Learn to use {{site.data.keyword.qiskit_runtime_notm}} using our examples and tutorials for one of the pre-built programs available for executing circuits efficiently.
      - **Standard**: A pay-as-you-go model for accessing IBM quantum systems. Build your own programs and leverage all the benefits of {{site.data.keyword.qiskit_runtime_notm}} by running on real quantum hardware.
   2. After completing the required information, click **Create**.

## Manage access to the service instance
{: #manage-access}

From the [{{site.data.keyword.cloud_notm}} console](/iam/overview){: external}, click Manage > Access (IAM) to create an IAM access policy, ideally an access group policy, to give users access to the service instance. Optionally use resource groups, access groups, tags, and so on, to manage resources and access to them.

For more information about roles, including details about program level roles and instructions to work with access groups, see the [IAM access documentation](https://cloud.ibm.com/docs/account?topic=account-userroles){: external}.

### Access roles
{: #access-roles}

Following are the roles you can assign to access groups:

Action | Description | Roles
---|---|---
quantum-computing.program.create | Create programs and change program privacy | Manager, Writer
quantum-computing.program.read | View programs and program details | Manager, Reader
quantum-computing.program.delete | Delete programs | Manager, Writer
quantum-computing.program.update | Update programs | Manager, Writer
quantum-computing.job.create | Run jobs | Manager, Writer
quantum-computing.job.read | View job results and logs | Manager, Reader
quantum-computing.job.delete | Delete jobs | Manager, Writer
{: caption="Table 1. Access roles to grant for managing, writing, and reading" caption-side="bottom"}

## Install Qiskit packages
{: #install-packages}

1. Install these packages.  They let you create circuits and work with primitive programs via {{site.data.keyword.qiskit_runtime_notm}}. For detailed instructions, refer to the [Qiskit textbook.](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. You need to keep these packages updated:

```Python
pip install qiskit
pip install qiskit-ibm-runtime
```
{: codeblock}

## Find your access credentials
{: #find-credentials}

Next, you will find your account credentials and authenticate with the service.

1. Find and copy your API key. From the [{{site.data.keyword.cloud_notm}} API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key. Your key will look something like this: `I9sxojrwurPrMWqNR_wc4rztMgVqE1HUmsfACMyw_G9n`
2. Find your Cloud Resource Name (CRN) or service instance name.
   - To find your CRN, from the {{site.data.keyword.cloud_notm}} console [Resource list page](https://cloud.ibm.com/resources){: external}, expand the "Services and software" section and look for your instance. Then click the row that contains your quantum service instance (don't click the name of the instance). In the pane that opens, click the icon to copy your CRN. For example:

      ```text
      crn:v1:bluemix:public:quantum-computing:us-east:a/b947c1c5a9378d64aed96696e4d76e8e:a3a7f181-35aa-42c8-94d6-7c8ed6e1a94b::
      ```

   - To find your service instance name, from the {{site.data.keyword.cloud_notm}} console [Resource list page](https://cloud.ibm.com/resources){: external}, expand the "Services and software" section and look for your instance.  The service instance name is in the **Name** column.

## Authenticate to the service
{: #authentication}

Call  `IBMRuntimeService` with your IBM Cloud API key and the CRN or Service name.

You can use the name of your service instance instead of the CRN.  You can also optionally save your credentials on disk (in the $HOME/.qiskit/qiskit-ibm.json file). By doing so, you only need to use IBMRuntimeService() in the future to initialize your account. If you don't save your credentials to disk, you have to run this command every time you start a new session.

```python
from qiskit_ibm_runtime import IBMRuntimeService

# Save account to disk.
IBMRuntimeService.save_account(auth="cloud", token=<IBM Cloud API key>, instance=<IBM Cloud CRN or Service instance name>)

service = IBMRuntimeService()
```
{: codeblock}

To update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials.  For more information about managing your account see the [account management tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/04_account_management.html){: external}.

For instructions to use the cloud Quantum Qiskit API, see the [authentication](/apidocs/quantum-computing#authentication){: external} section in the API documentation.

## Test your setup
{: #test-setup}

Run the Hello World program to ensure that your environment is set up properly:

```Python
from qiskit.test.reference_circuits import ReferenceCircuits
from qiskit_ibm_runtime import IBMRuntimeService

service = IBMRuntimeService()
program_inputs = {'iterations': 1}
options = {"backend_name": "ibmq_qasm_simulator"}
job = service.run(program_id="hello-world",
                  options=options,
                  inputs=program_inputs
                 )
print(f"job id: {job.job_id}")
result = job.result()
print(result)
```
{: codeblock}

Result:

```text
All done!
```

## Create a circuit
{: #create-circuit}

You'll need one or more circuits to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.


## Choose a program to run
{: #choose-program}

The list of available programs is on the overview page for your quantum service instance.  from the {{site.data.keyword.cloud_notm}} console [Resource list page](https://cloud.ibm.com/resources){: external}, expand the "Services and software" section and look for your instance. Next, click the name of your quantum service instance. Scroll down to see the available programs.

The following programs are publicly available. For more information about these programs, including the required input parameters, see the [Estimator](/docs/quantum-computing?topic=quantum-computing-program-estimator) or [Sampler](/docs/quantum-computing?topic=quantum-computing-program-sampler) topic.

- **Estimator**:  
       Returns an observable expectation value generated by given circuits executed on the target backend.
- **Sampler**:  
       Sample distributions generated by given circuits executed on the target backend.

You can also view all available programs by using the [List programs](/apidocs/quantum-computing#list-programs){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/list_programs){: external}.


## Choose a backend
{: #choose-backend}

Before running a job, you can optionally choose a backend (a physical quantum system or a simulator) to run on.  If you do not specify one, the job is sent to the least busy device that you have access to.

The Standard plan only allows access to physical quantum systems, while the Lite plan only allows access to simulators.
{: #note}

To find your available backends, run `service.backends()` in Qiskit and note the name of the backend you want to use.  For full details, including available options, see the [Qiskit Runtime API documentation](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.IBMRuntimeService.backends.html#qiskit_ibm_runtime.IBMRuntimeService.backends).

You can also view your available backends by using the [List your devices](/apidocs/quantum-computing#list-devices){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/list-devices){: external}.


## Run the job
{: #run-job-step}

You will use the {{site.data.keyword.qiskit_runtime_notm}} IBMRuntimeService.run() method, which takes the following parameters:

- program_id: ID of the program to run.
- inputs: Program input parameters. These input values are passed to the runtime program and are dependent on the parameters defined for the program.
- options: Runtime options. These options control the execution environment. The current  available options are backend_name and log_level, which are optional.

   If you do not specify a backend, the job is sent to the least busy device that you have access to. For full details about the available options, see the [API guide](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.RuntimeOptions.html#qiskit_ibm_runtime.RuntimeOptions){: external}.

- result_decoder: Optional class used to decode the job result.

In the following example, we will submit a circuit to the Sampler program.

```python
from qiskit.test.reference_circuits import ReferenceCircuits
from qiskit_ibm_runtime import IBMRuntimeService

service = IBMRuntimeService()
program_inputs = {
    'circuits': ReferenceCircuits.bell()
}
options = {'backend_name': 'ibmq_qasm_simulator'}
job = service.run(
      program_id="sampler",
      options=options,
      inputs=program_inputs)
print(f"job ID: {job.job_id}")
result = job.result()
```
{: codeblock}

If you do not specify the device, the job is sent to the least busy device that you have access to.

To ensure fairness, there is a maximum execution time for each {{site.data.keyword.qiskit_runtime_notm}} job. If a job exceeds this time limit, it is forcibly terminated. The maximum execution time is the smaller of the system limit and the `max_execution_time` defined by the program. The system limit is 3 hours for jobs running on a simulator and 8 hours for jobs running on a physical system.
{: note}

You can also run a job by using the [Create job](/apidocs/quantum-computing#create-job){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/create-job){: external}.


## (Optional) Return the job status
{: #return-status}

Follow up the {{site.data.keyword.qiskit_runtime_notm}} IBMRuntimeService.run() method by running a RuntimeJob method. The run() method returns a RuntimeJob instance, which represents the asynchronous execution instance of the program.

There are several RuntimeJob methods to choose from, including job.status():

```python
job.status()
```
{: codeblock}

You can also return the job status by using the [List job details](/apidocs/quantum-computing#get-job-details-jid){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/get-job-details-jid){: external}.

## Next steps
{: #next-steps}

- View the [API reference](/apidocs/quantum-computing){: external}.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}

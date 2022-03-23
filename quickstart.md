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


# Getting started
{: #quickstart}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial walks you through the steps to set up a {{site.data.keyword.qiskit_runtime_notm}} service instance, log into your service instance, and run your first job on a quantum computer.
{: shortdesc}


## Create a service instance
{: #create-configure}
{: step}

If you already created your {{site.data.keyword.qiskit_runtime_notm}} service instance, skip to the next step.

1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, select the Create tab, then choose the appropriate service plan, depending on what you need access to:
      - **Lite**: Free simulators-only plan to help you get started with {{site.data.keyword.qiskit_runtime_notm}}. Learn to use {{site.data.keyword.qiskit_runtime_notm}} using our examples and tutorials for one of the pre-built programs available for executing circuits efficiently.
      - **Standard**: A pay-as-you-go model for accessing IBM quantum systems. Build your own programs and leverage all the benefits of {{site.data.keyword.qiskit_runtime_notm}} by running on real quantum hardware.
2. After completing the required information, click **Create**.

## Install Qiskit packages
{: #install-packages}
{: step}

Install these packages.  They let you create circuits and work with primitive programs via {{site.data.keyword.qiskit_runtime_notm}}. For detailed instructions, refer to the [Qiskit textbook.](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. You need to keep these packages updated:

```Python
pip install qiskit
```
{: codeblock}

```Python
pip install qiskit-ibm-runtime
```
{: codeblock}

## Find your access credentials
{: #find-credentials}
{: step}

Next, you will find your account credentials and authenticate with the service.

1. Find and copy your API key. From the [API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key.
2. Find your service instance name or Cloud Resource Name (CRN).
   - To find your service instance name, open the [Instances page](https://cloud.ibm.com/quantum/instances){: external} and find your instance.  The service instance name is in the **Name** column.
   - To find your CRN, open the [Instances page](https://cloud.ibm.com/quantum/instances){: external} and click your instance. In the page that opens, click the icon to copy your CRN.

## Authenticate to the service
{: #authentication}
{: step}

To authenticate to the service, call  `IBMRuntimeService` with your IBM Cloud API key and the CRN or Service name. You can also optionally save your credentials on disk (in the $HOME/.qiskit/qiskit-ibm.json file).  If you don't save your credentials to disk, you have to run this command every time you start a new session.

If you save your credentials to disk, you only need to use `IBMRuntimeService()` in the future to initialize your account.
{: note}

```python
from qiskit_ibm_runtime import IBMRuntimeService

# Save account to disk.
IBMRuntimeService.save_account(auth="cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN or Service instance name>")

service = IBMRuntimeService()
```
{: codeblock}

If you need to update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials.  For more information about managing your account see the [account management tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/04_account_management.html){: external}.

For instructions to use the cloud Quantum Qiskit API, see the [authentication](/apidocs/quantum-computing#authentication){: external} section in the API documentation.

## Test your setup
{: #test-setup}
{: step}

Run the Hello World program to ensure that your environment is set up properly.

If you did not save your credentials to disk, specify `IBMRuntimeService.save_account(auth="cloud", token=<IBM Cloud API key>, instance=<IBM Cloud CRN or Service instance name>)
` instead of `IBMRuntimeService()` in the following code.

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

## Choose a program to run
{: #choose-program}
{: step}

Qiskit Runtime uses [primitive programs](/docs/quantum-computing?topic=quantum-computing-overview) to interface with quantum computers. The following programs are publicly available. Choose the appropriate link to continue learning how to run a program.

- **[Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler)**:  
       Sample distributions generated by given circuits executed on the target backend.
- **[Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator)**:  
       Returns an observable expectation value generated by given circuits executed on the target backend.

## Next steps
{: #next-steps}

- View the [API reference](/apidocs/quantum-computing){: external}.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}

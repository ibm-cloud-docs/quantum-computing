<partner>---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-30"

keywords: quantum, Qiskit, runtime, near time compute, channel partner, reseller, 3rd party, vendor, b2b

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}

# Getting started
{: #quickstart-partner}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial walks you through the steps to set up a {{site.data.keyword.qiskit_runtime_notm}} service instance, log in to your service instance, and run your first job on a quantum computer.
{: shortdesc}

## Before you begin
{: #before-partner}

The IBM Quantum team will grant you access to the Channel Partner plan.  After being notified that you have been granted access, continue to the next step.

## Create a service instance
{: #create-configure-partner}
{: step}

1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, choose the Channel Partner service plan.  If you don't see this plan, contact your IBM Quantum representative. 

2. Complete the required information, then click **Create**.

## Install or update Qiskit packages
{: #install-packages-partner}
{: step}

Install or update the following packages in your development environment. They let you create circuits and work with primitive programs with {{site.data.keyword.qiskit_runtime_notm}}. For detailed instructions, refer to the [Qiskit textbook](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. Periodically check the [Qiskit release notes](https://qiskit.org/documentation/release_notes.html) (or rerun these commands) so that you always have the latest version.

Be sure to run these commands even if you already installed the packages, to ensure that you have the latest versions.
{: note}

```Python
# Installs the latest version of the Qiskit meta-package for circuit creation.
pip install qiskit -U
```
{: codeblock}

```Python
# Installs the latest version of the Qiskit Runtime package, which is needed to interact with the Qiskit Runtime primitives on IBM Cloud.
pip install qiskit-ibm-runtime -U
```
{: codeblock}

## Authenticate to the service
{: #authentication-partner}
{: step}

To authenticate to the service, call `QiskitRuntimeService` with your IBM Cloud API key and the CRN: 

```python
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>")
```
{: codeblock}

### Find your access credentials
{: #find-credentials-partner}

1. Find your API key. From the [API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key, then copy it to a secure location so you can use it for authentication.
2. Find your Cloud Resource Name (CRN). Open the [Instances page](https://cloud.ibm.com/quantum/instances){: external} and click your instance. In the page that opens, click the icon to copy your CRN. Save it in a secure location so you can use it for authentication.


## Optionally save your credentials to disk
{: #save-to-disk-partner}
{: step}

Optionally save your credentials to disk (in the `$HOME/.qiskit/qiskit-ibm.json` file). If you don't save your credentials to disk, you must specify your credentials every time you start a new session.

If you save your credentials to disk, you can use `QiskitRuntimeService()` in the future to initialize your account.

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Save account to disk.
QiskitRuntimeService.save_account(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>")

service = QiskitRuntimeService()
```
{: codeblock}

If you need to update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials. For more information about managing your account, see the [account management topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/how_to/account-management.html){: external}.

For instructions to use the cloud Quantum Qiskit API, see the [authentication](/apidocs/quantum-computing#authentication){: external} section in the API documentation.

## Test your setup
{: #test-setup-partner}
{: step}

Run the Hello World program to ensure that your environment is set up properly.

If you did not save your credentials to disk, specify `QiskitRuntimeService(channel="ibm_cloud", token=<IBM Cloud API key>, instance=<IBM Cloud CRN>)`
instead of `QiskitRuntimeService()` in the following code.

```Python
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService()
program_inputs = {'iterations': 1}
options = {"backend": ""}
job = service.run(program_id="hello-world",
                  options=options,
                  inputs=program_inputs
                 )
print(f"job id: {job.job_id()}")
result = job.result()
print(result)
```
{: codeblock}

Result:

```text
Hello world!
```

## Choose a program to run
{: #choose-program-partner}
{: step}

Qiskit Runtime uses [primitive programs](/docs/quantum-computing?topic=quantum-computing-overview#primitive-programs) to interface with quantum computers. The following programs are publicly available. Choose the appropriate link to continue learning how to run a program.

- **[Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler)**:  
       Allows a user to specify a circuit as an input and then generate quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.
- **[Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator)**:  
       Allows a user to specify a list of circuits and observables and selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input. It is designed to enable users to efficiently calculate and interpret expectation values of quantum operators that are required for many algorithms. 

</draft>

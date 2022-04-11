---

copyright:
  years: 2021, 2022
lastupdated: "2022-04-11"

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

If you already created your {{site.data.keyword.qiskit_runtime_notm}} service instance, skip to the next step.  To determine whether you already have an instance, check your [IBM Cloud Instances page](https://cloud.ibm.com/quantum/instances). If you have one or more instances listed, you can skip ahead to [Install Qiskit packages](#install-packages).

![This image shows an Instances page with two instances.](images/instances.png "Instances page with two instances"){: caption="Figure 1. Instances page showing multiple instances" caption-side="bottom"}


1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, choose the appropriate service plan, depending on what you need access to. For more information about these plans, see the [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost) topic.

      - **Lite**: Free simulators-only plan to help you get started with {{site.data.keyword.qiskit_runtime_notm}}. Learn to use {{site.data.keyword.qiskit_runtime_notm}} using our examples and tutorials for one of the pre-built programs available for executing circuits efficiently.
      - **Standard**: A pay-as-you-go model for accessing IBM Quantum systems. Build your own programs and leverage all the benefits of {{site.data.keyword.qiskit_runtime_notm}} by running on real quantum hardware.

      If you want to access physical devices as well as simulators, you will need to set up one instance with the Lite plan and one instance with the Standard plan.
      {: note}

2. After completing the required information, click **Create**.

## Install Qiskit packages
{: #install-packages}
{: step}

Install the following packages to your development environment.  They let you create circuits and work with primitive programs via {{site.data.keyword.qiskit_runtime_notm}}. For detailed instructions, refer to the [Qiskit textbook](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. Make sure to periodically check the [Qiskit release notes](https://qiskit.org/documentation/release_notes.html) to ensure that you always have the latest version.

```Python
# Installs the Qiskit meta-package for circuit creation.
pip install qiskit
```
{: codeblock}

```Python
# Installs the Qiskit Runtime package, which is needed to interact with the Qiskit Runtime primitives on IBM Cloud.
pip install qiskit-ibm-runtime
```
{: codeblock}

## Authenticate to the service
{: #authentication}
{: step}

To authenticate to the service, call `QiskitRuntimeService` with your IBM Cloud API key and the CRN:

```python
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>")
```
{: codeblock}

### Find your access credentials
{: #find-credentials}

1. Find your API key. From the [API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key, then copy it to a secure location so you can use it for authentication.
2. Find your Cloud Resource Name (CRN). Open the [Instances page](https://cloud.ibm.com/quantum/instances){: external} and click your instance. In the page that opens, click the icon to copy your CRN. Save it in a secure location so you can use it for authentication.


## Optionally save your credentials to disk
{: #save-to-disk}
{: step}

Optionally save your credentials to disk (in the `$HOME/.qiskit/qiskit-ibm.json` file). If you don't save your credentials to disk, you have to specify your credentials every time you start a new session.

If you save your credentials to disk, you can use `QiskitRuntimeService()` in the future to initialize your account.

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Save account to disk.
QiskitRuntimeService.save_account(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>")

service = QiskitRuntimeService()
```
{: codeblock}

If you need to update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials.  For more information about managing your account see the [account management tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/04_account_management.html){: external}.

For instructions to use the cloud Quantum Qiskit API, see the [authentication](/apidocs/quantum-computing#authentication){: external} section in the API documentation.

## Test your setup
{: #test-setup}
{: step}

Run the Hello World program to ensure that your environment is set up properly.

If you did not save your credentials to disk, specify `QiskitRuntimeService(channel="ibm_cloud", token=<IBM Cloud API key>, instance=<IBM Cloud CRN>)`
instead of `QiskitRuntimeService()` in the following code.

```Python
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService()
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
Hello world!
```

## Choose a program to run
{: #choose-program}
{: step}

Qiskit Runtime uses [primitive programs](/docs/quantum-computing?topic=quantum-computing-overview) to interface with quantum computers. The following programs are publicly available. Choose the appropriate link to continue learning how to run a program.

- **[Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler)**:  
       Allows a user to specify a circuit as an input and then generate an error-mitigated readout of quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.
- **[Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator)**:  
       Allows a user to specify a list of circuits and observables and provides the ability to selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input. It is designed to enable users to efficiently calculate and interpret expectation values of quantum operators required for many algorithms. 

## Next steps
{: #next-steps}

- View the [API reference](/apidocs/quantum-computing){: external}.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}.

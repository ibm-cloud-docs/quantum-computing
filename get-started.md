---

copyright:
  years: 2021, 2023
lastupdated: "2022-10-13"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}


# Getting started
{: #get-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial walks you through the steps to set up a {{site.data.keyword.qiskit_runtime_notm}} service instance, log in to your service instance, and run your first job on a quantum computer.
{: shortdesc}

- If you are an entity upgrading from an IBM Quantum program who needs to set up Qiskit Runtime, refer to [Upgrade from the Open plan](/docs/quantum-computing?topic=quantum-computing-upgrade-from-open) for instructions to set up a cloud account, a service instance, and work with users.
- If your organization has a more complex structure, refer to [Plan Qiskit Runtime for an organization](/docs/quantum-computing?topic=quantum-computing-quickstart-org) for more flexible instructions to set up a service instance and work with users.
- If you are an individual setting up the {{site.data.keyword.qiskit_runtime_notm}} service for the first time or if you have been invited to an instance by an administrator, continue following the steps in this topic. 

## Create a service instance
{: #create-configure}
{: step}

If you already created a {{site.data.keyword.qiskit_runtime_notm}} service instance or were invited to one by an administrator, skip to the next step. To determine whether you already have access to an instance, check your [IBM Cloud Instances page](https://cloud.ibm.com/quantum/instances){: external}. If you have one or more instances shown, you can skip ahead to [Install Qiskit packages](#install-packages).

![This image shows an Instances page with two instances.](images/instances.png "Instances page with two instances"){: caption="Figure 1. Instances page showing multiple instances" caption-side="bottom"}

1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, choose the appropriate service plan, depending on what you need access to. For more information about these plans, see the [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-plans) topic.

      - **Lite**: Free simulators-only plan to help you get started with {{site.data.keyword.qiskit_runtime_notm}}. Learn to use {{site.data.keyword.qiskit_runtime_notm}} by following our examples and tutorials for one of the pre-built programs available for running circuits efficiently.
      - **Standard**: A pay-as-you-go model for accessing IBM Quantum systems and simulators. Build your own programs and leverage all the benefits of {{site.data.keyword.qiskit_runtime_notm}} by running on real quantum hardware, while maintaining access to all of the simulators available to the Lite plan. This plan includes access to Q-CTRL Automated Error Suppression at no additional cost. 

         Because this is not a free plan, it is important to understand how to best manage your costs. See [Manage the cost](/docs/quantum-computing?topic=quantum-computing-cost) for tips to limit your cost, how to set up spending notifications, and more.

         To learn more about Q-CTRL, refer to the [Q-CTRL documentation.](https://docs.q-ctrl.com/q-ctrl-embedded){: external}.

2. Complete the required information, then click **Create**.
   
   To use Q-CTRL Automated Error Suppression, choose it as your **Performance management strategy** on this page. 
   {: note}

## Install or update Qiskit packages
{: #install-packages}
{: step}

Install or update the following packages in your development environment. They let you create circuits and work with primitive programs with {{site.data.keyword.qiskit_runtime_notm}}. For detailed instructions, refer to the [Qiskit install and setup topic](https://docs.quantum-computing.ibm.com/start/install){: external}. Periodically check the [Qiskit release notes](https://qiskit.org/documentation/release_notes.html) (or rerun these commands) so that you always have the latest version.

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
{: #authentication}
{: step}

To authenticate to the service, call `QiskitRuntimeService` with your IBM Cloud API key and the CRN:

```python
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>")
```
{: codeblock}

If you set up your instance to include Q-CTRL performance management, when initializing the `QiskitRuntimeService()`, include the additional argument `channel_strategy="q-ctrl"`. To learn more about Q-CTRL performance management strategy visit the [Q-CTRL documentation](https://docs.q-ctrl.com/q-ctrl-embedded).
{: note}

### Find your access credentials
{: #find-credentials}

1. Find your API key. From the [API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key, then copy it to a secure location so you can use it for authentication.
2. Find your Cloud Resource Name (CRN). Open the [Instances page](https://cloud.ibm.com/quantum/instances){: external} and click your instance. In the page that opens, click the icon to copy your CRN. Save it in a secure location so you can use it for authentication.


## Optionally save your credentials to disk
{: #save-to-disk}
{: step}

Optionally save your credentials to disk (in the `$HOME/.qiskit/qiskit-ibm.json` file). If you don't save your credentials to disk, you must specify your credentials every time you start a new session.

If you save your credentials to disk, you can use `QiskitRuntimeService()` in the future to initialize your account.

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Save account to disk and save it as the default.
QiskitRuntimeService.save_account(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>", name="account-name", set_as_default=True)

# Load the saved credentials 
service = QiskitRuntimeService(name="account-name")
```
{: codeblock}

If you need to update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials. For more information about managing your account, see the [account management topic](https://docs.quantum-computing.ibm.com/run/account-management){: external}.

For instructions to use the cloud Quantum Qiskit API, see the [authentication](/apidocs/quantum-computing#authentication){: external} section in the API documentation.

## Test your setup
{: #test-setup}

Run a simple circuit using `Sampler` to ensure that your environment is set up properly:

```python

from qiskit import QuantumCircuit

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

from qiskit_ibm_runtime import QiskitRuntimeService,  Sampler

service = QiskitRuntimeService()
sampler = Sampler(service=service, backend="ibm_gotham")
job = sampler.run(qc)
result = job.result()
```
{: codeblock}

## Run a program
{: #choose-program}
{: step}

Qiskit Runtime uses [primitive programs](/docs/quantum-computing?topic=quantum-computing-overview#primitive-programs) to interface with quantum computers. The following programs are publicly available. 

- **Sampler**:  
       Allows a user to specify a circuit as an input and then generate quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.
- **Estimator**:  
       Allows a user to specify a list of circuits and observables and selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input. It is designed to enable users to efficiently calculate and interpret expectation values of quantum operators that are required for many algorithms. 

To ensure faster and more efficient results, as of 1 March 2024, circuits and observables need to be transformed to only use instructions supported by the system (referred to as *instruction set architecture (ISA)* circuits and observables) before being submitted to the Qiskit Runtime primitives. See the [transpilation documentation](https://docs.quantum.ibm.com/transpile){: external} for instructions to transform circuits. Due to this change, the primitives will no longer perform layout or routing operations. Consequently, transpilation options referring to those tasks will no longer have any effect. Users can still request that the primitives do no optimization of input circuits by using `options.transpilation.skip_transpilation`.
{: important}

This example uses the Sampler primitive:

```python
# Prepare the input circuit.
from qiskit import QuantumCircuit

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()

# Execute the circuit
from qiskit_ibm_runtime import Sampler

backend = service.backend("ibmq_qasm_simulator")
sampler = Sampler(backend)
job = sampler.run([(circuits=bell)])
result = job.result()

print(job.result())
```
{: codeblock}

## Next steps
{: #next-steps-getstarted}

- View the [API reference](/apidocs/quantum-computing){: external}.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}.


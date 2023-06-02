---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-22"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, IBM Quantum Platform

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Migrate your setup from ``qiskit-ibmq-provider``
{: #migrate-setup}

This guide describes how to migrate code from the legacy IBMQ provider (`qiskit-ibmq-provider`) package to use Qiskit Runtime (`qiskit-ibm-runtime`). This guide includes instructions to migrate legacy runtime programs to the new syntax. However, the ability to use custom uploaded programs is pending deprecation, so these should be migrated to use primitives instead.  

{: shortdesc}

For further details about migrating from `qiskit-ibmq-provider` to `qiskit-ibm-runtime`, see the [Migration guide](/docs/quantum-computing?topic=quantum-computing-migrate-overview).

## Import path
{: #migrate-import-path}

The import path has changed as follows:

**Legacy**

``` python
from qiskit import IBMQ

```
{: codeblock}

**Updated**

``` python
from qiskit_ibm_runtime import QiskitRuntimeService

```
{: codeblock}    


## Save and load accounts
{: #migrate-import-path}

Use the updated code to work with accounts.

**Legacy - Save accounts**

``` python
IBMQ.save_account("<QUANTUM_TOKEN>", overwrite=True)

```
{: codeblock}  

**Updated - Save accounts**
The new syntax accepts credentials for two different channels. For more information on retrieving account credentials, see the `getting started guide <https://qiskit.org/documentation/partners/qiskit_ibm_runtime/getting_started.html>`_.

``` python
	# IBM cloud channel
    QiskitRuntimeService.save_account(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>", overwrite=True)

    # IBM quantum channel
    QiskitRuntimeService.save_account(channel="ibm_quantum", token="<QUANTUM_TOKEN>", overwrite=True)

```
{: codeblock}

**Legacy - Load accounts**

``` python
IBMQ.load_account()

```
{: codeblock} 

**Updated - Load accounts**

The new syntax combines the functionality from ``load_account()`` and ``get_provider()`` in one statement. The ``channel`` input parameter is optional. If multiple accounts have been saved in one device and no ``channel`` is provided, the default is ``"ibm_cloud"``.

``` python
# To access saved credentials for the IBM cloud channel
service = QiskitRuntimeService(channel="ibm_cloud")

# To access saved credentials for the IBM quantum channel
service = QiskitRuntimeService(channel="ibm_quantum")

```
{: codeblock}   

## Channel selection (get a provider)
{: #migrate-setup-channel} 

Use the updated code to select a channel.

**Legacy**

``` python
provider = IBMQ.get_provider(project="my_project", group="my_group", hub="my_hub")

```
{: codeblock}

**Updated**

The new syntax combines the functionality from ``load_account()`` and ``get_provider()`` in one statement.
When using the ``ibm_quantum`` channel, the ``hub``, ``group``, and ``project`` are specified through the new
``instance`` keyword.

``` python
# To access saved credentials for the IBM cloud channel
service = QiskitRuntimeService(channel="ibm_cloud")

# To access saved credentials for the IBM quantum channel and select an instance
service = QiskitRuntimeService(channel="ibm_quantum", instance="my_hub/my_group/my_project")

```
{: codeblock} 

## Get the backend
{: #migrate-get-backend} 

Use the updated code to view backends.

**Legacy**

``` python
backend = provider.get_backend("ibmq_qasm_simulator")

```
{: codeblock}

**Updated**

``` python
backend = service.backend("ibmq_qasm_simulator")

```
{: codeblock} 

## Upload, view, or delete custom prototype programs
{: #migrate-upload-program}

To work with custom programs, replace ``provider.runtime`` with ``service``.

This function is pending deprecation.
{: note}

**Legacy**

``` python
# Printing existing programs
provider.runtime.pprint_programs()

# Deleting custom program
provider.runtime.delete_program("my_program") # Substitute "my_program" with your program ID

# Uploading custom program
program_id = provider.runtime.upload_program(
             data=program_data,
             metadata=program_json
             )

```
{: codeblock} 

**Updated**

``` python
# Printing existing programs
service.pprint_programs()

# Deleting custom program
service.delete_program("my_program") # Substitute "my_program" with your program ID

# Uploading custom program
program_id = service.upload_program(
             data=program_data,
             metadata=program_json
             )

```
{: codeblock} 

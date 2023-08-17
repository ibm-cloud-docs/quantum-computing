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

### Legacy
{: #migrate-import-path-legacy}

``` python
from qiskit import IBMQ

```
{: codeblock}

### Updated
{: #migrate-import-path-updated}

``` python
from qiskit_ibm_runtime import QiskitRuntimeService

```
{: codeblock}    


## Save and load accounts
{: #migrate-save-accounts}

Use the updated code to work with accounts.

### Legacy - Save accounts
{: #migrate-save-accounts-legacy}

``` python
IBMQ.save_account("<QUANTUM_TOKEN>", overwrite=True)

```
{: codeblock}  

### Updated - Save accounts
{: #migrate-save-accounts-updated}

The new syntax accepts credentials for two different channels. For more information on retrieving account credentials, see the `getting started guide <https://docs.quantum-computing.ibm.com/start>`_.

``` python
	# IBM cloud channel
    QiskitRuntimeService.save_account(channel="ibm_cloud", token="<IBM Cloud API key>", instance="<IBM Cloud CRN>", overwrite=True)

    # IBM quantum channel
    QiskitRuntimeService.save_account(channel="ibm_quantum", token="<QUANTUM_TOKEN>", overwrite=True)

```
{: codeblock}

### Legacy - Load accounts
{: #migrate-load-accounts-legacy}

``` python
IBMQ.load_account()

```
{: codeblock} 

### Updated - Load accounts
{: #migrate-load-accounts-updated}


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

### Legacy
{: #migrate-setup-channel-legacy} 

``` python
provider = IBMQ.get_provider(project="my_project", group="my_group", hub="my_hub")

```
{: codeblock}

### Updated
{: #migrate-setup-channel-updated} 

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

### Legacy
{: #migrate-get-backend-l} 


``` python
backend = provider.get_backend("ibmq_qasm_simulator")

```
{: codeblock}

### Updated
{: #migrate-get-backend-u} 


``` python
backend = service.backend("ibmq_qasm_simulator")

```
{: codeblock} 

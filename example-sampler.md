---

copyright:
  years: 2021
lastupdated: "2022-02-15"

keywords: quantum, Qiskit, runtime, near time compute, sampler, primitive

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Get started with the Sampler primitive
{: #example-sampler}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

Learn how to set up and use the Sampler primitive program.
{: shortdesc}

## Overview
{: #sampler-overview}

The Sampler primitive lets you more accurately contextualize counts. It takes a user circuit as an input and generates an error mitigated readout of quasiprobabilities. This enables you to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.  


## Before you begin
{: #example-sampler-byb}

1. Follow the steps in [Creating and configuring a Quantum Service instance](/docs/quantum-computing?topic=quantum-computing-gettingstarted) and [Get ready to work with your Quantum Service instance](/docs/quantum-computing?topic=quantum-computing-access) to get your Quantum Service instance ready to use.

2. You'll need at least one circuit to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.


## Inputs for Sampler
{: #sampler-inputs}
{: step}

The sample takes in
* The **circuits** you want to investigate.
* The **parameter** inputs to evaluate the circuits.
* (optional) The **backend** to use.  If one is not specified, the least busy backend that you have access to is used.

For example:

   ```Python
   from qiskit import QuantumCircuit
   from qiskit_ibm_runtime import IBMRuntimeService

   bell = QuantumCircuit(2, 2)
   bell.h(0)
   bell.cx(0, 1)
   bell.measure(0, 0)
   bell.measure(1, 1)


   θ = [0, 1, 1, 2, 3, 5]
   ```

## Run a sampler job


```Python
     from qiskit_ibm_runtime import IBMRuntimeService

     service = IBMRuntimeService(auth="cloud", instance=<IBM Cloud CRN>)

     program_inputs =  {
     'circuits': [bell],
     'parameters': [θ],
     'shots' : 1024,
     'backend': "ibm_canberra"
   }

   job = service.run(program_id="sampler",
                 inputs=program_inputs
                )
   print(f"job id: {job.job_id}")
   bell_result = job.result()
   print("bell results", bell_result)

```

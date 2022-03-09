---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-07"

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


## Prepare the environment
{: #example-sampler-byb}
{: step}

1. Follow the steps in the [quick start guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your Quantum Service instance ready to use.
2. You'll need at least one circuit to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

## Specify program inputs
{: #sampler-inputs}
{: step}

The Sampler takes in:
* The **circuits** you want to investigate.
* The **parameters** input to evaluate the circuits.
* Optional: Other **run_options**, such as how many **shots** to run.
* Optional: The instruction to **skip_transpilation**.

Example:

```Python
from qiskit import QuantumCircuit
from qiskit_ibm_runtime import IBMRuntimeService

bell = QuantumCircuit(2, 2)
bell.h(0)
bell.cx(0, 1)
bell.measure(0, 0)
bell.measure(1, 1)

program_inputs =  {
'circuits': [bell],
'run_options': {
  'shots': 1024
}
}
```
{: codeblock}

## Program options    
{: #sampler-options}
{: step}

Specify other program options, such as the **backend name**.  If one is not specified, the least busy backend is used.

Example:

```Python
options = {
'backend_name': "ibmq_qasm_simulator"
}

```
{: codeblock}

## Run the job
{: #sampler-run}
{: step}

Run the job; specifying your previously defined inputs and options:

```Python
job = service.run(program_id="sampler",
inputs=program_inputs,
options=options
)
```
{: codeblock}

## Get results
{: #sampler-results}
{: step}

Request the outputs you are interested in. You will need the job ID if you want to troubleshoot or look up the results again later.

```Python
print(f"job id: {job.job_id}")
bell_result = job.result()
print("bell results", bell_result)
```
{: codeblock}

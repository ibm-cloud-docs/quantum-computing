---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-07"

keywords: quantum, Qiskit, runtime, near time compute, estimator, primitive

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}


# Get started with the Estimator primitive
{: #example-estimator}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

Learn how to set up and use the Estimator primitive program.
{: shortdesc}

## Overview
{: #estimator-overview}

The Estimator primitive lets you efficiently calculate and interpret expectation values of quantum operators required for many algorithms. You can specify a list of circuits and observables, then evaluate expectation values and variances for a given parameter input.  


## Prepare the environment
{: #example-estimator-byb}
{: step}

1. Follow the steps in the [quick start guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your quantum service instance ready to use.

2. You'll need at least one circuit to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

3. Create a list of observables. Observables let you define the properties of the circuit that are relevant to your problem and enable you to efficiently measure their expectation value. For simplicity, you can use the [PauliSumOp class](https://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.html#module-qiskit.opflow.primitive_ops) in Qiskit to define them.

## Prepare inputs
{: #Estimator-inputs}
{: step}

Estimator lets you define your jobs by using the following inputs to calculate expectation values.

These inputs can also be inspected by running the following commands:

```Python
from qiskit_ibm_runtime import IBMRuntimeService

IBMRuntimeService.save_account(auth="cloud", token=<API Token>, instance=<IBM Cloud CRN or Service Name>)
service = IBMRuntimeService()

program = service.program("estimator")
print(program)
```
  {: codeblock}


* The **circuits** you want to investigate.
* The **observables** you want applied to the circuits.
* The **parameters** input to evaluate the circuits.
* Optional: Other **run_options**, such as how many **shots** to run.


### Example of preparing the required inputs:

```Python
from qiskit.opflow import PauliSumOp
from qiskit.circuit.library import RealAmplitudes
from qiskit_ibm_runtime import IBMRuntimeService

service = IBMRuntimeService(auth="cloud", token="token", instance="crn:v1:staging:public:quantum-computing:us-east:a/78d9efd23fdb4894837f663626efb744:7d4e1bd0-b3e1-415b-b601-5fc8b38de8b2::", url="https://cloud.ibm.com")

observable = PauliSumOp.from_list(
    [
        ("II", -1.052373245772859),
        ("IZ", 0.39793742484318045),
        ("ZI", -0.39793742484318045),
        ("ZZ", -0.01128010425623538),
        ("XX", 0.18093119978423156),
    ]
)
ansatz = RealAmplitudes(num_qubits=2, reps=2)
parameters = [0, 1, 1, 2, 3, 5]
run_options = {"shots": 1000}


program_inputs = {
    "circuits": ansatz,
    "observables": observable,
    "parameters": parameters,
    "run_options": run_options
}
```
{: codeblock}

### Prepare optional Inputs:
{: #estimator-optional}
{: step}

For a given parameter input for your circuit, you can use these optional parameters that allow you to optimize your job execution.

The **backend** lets you optionally specify the backend to run on.  If you do not specify one, the least busy backend is used.

```Python
options = {"backend_name": "ibmq_qasm_simulator"}
```
{: codeblock}

## Run the job

After preparing all of the input parameters, submit the job.  The values and variances are returned; along with the actual number of shots run:
Example:

```Python
job = service.run(program_id="estimator",
                       options=options,
                       inputs=program_inputs,
                       )
print(f"Job ID: {job.job_id}")
print(job.result())
```
{: codeblock}

Output:

```text
{'values': array([-1.30777957]), 'variances': array([0.29598401]), 'shots': 1000}
```

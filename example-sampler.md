---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-05"

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

The Sampler primitive lets you more accurately contextualize counts. It takes a user circuit as an input and generates quasiprobabilities. This enables you to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.  

If you are using the Standard plan, running Sampler incurs a cost. See [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost) for cost information.
{: note}

## Prepare the environment
{: #example-sampler-byb}
{: step}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your Quantum Service instance ready to use.
2. You'll need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

## Start a session
{: #start-session-sampler-example}
{: step}

With Qiskit Runtime primitives, we introduce the concept of a session that allows you to define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data you send so it doesn't have to be transmitted to the quantum data center on each iteration.

### Specify program inputs
{: #sampler-inputs}
{: step}

The Sampler takes in:
* The **circuits** you want to investigate.
* The **service** to use.

* The **parameters** input to evaluate the circuits.
* Optional: **options**, such as the backend to run on.  If a backend is not specified, the least busy backend is used. To learn about choosing a backend, see [Choose a system or simulator](/docs/quantum-computing?topic=quantum-computing-choose-backend).
* Optional: The instruction to **skip_transpilation**.

Example:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler
from qiskit import QuantumCircuit

service = QiskitRuntimeService(channel="ibm_cloud", token="<api-token>", instance="<IBM Cloud CRN>")

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()
```
{: codeblock}

## Write to & read from a session
{: #rw-session-sampler-example}
{: step}

Running a job and returning the results are done by writing to and reading from the session. The session closes when the code exits the `with` block.

### Run the job & print results
{: #sampler-run}
{: step}

Run the job, specifying your previously defined inputs and options. In this simple example, there is only one circuit and it does not have parameters.

In each call, you will use `circuit_indices` to specify which circuit to run and, if applicable,  `parameter_values` specifies which parameter to use with the specified circuit.

In this example, we specified one circuit, `circuits=bell`, and we asked for the result for running the first (and only) circuit: `circuit_indices=[0]`.

As you will see in later examples, if we had specified multiple circuits, such as `circuits=[pqc, pqc2]`, you could specify `circuit_indices=[1]` to run the `pqc2` circuit.


```Python
# executes a Bell circuit
with Sampler(circuits=bell, service=service, options={ "backend": "" }) as sampler:
    result = sampler(circuit_indices=[0], shots=1024)
    print(result)
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{'00': 0.4873046875, '11': 0.5126953125}], metadata=[{'header_metadata': None, 'shots': 1024}])
```

## Multiple circuit example
{: #example2-sampler}

In this example, we specify three circuits, but they have no parameters:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler
from qiskit import QuantumCircuit

service = QiskitRuntimeService(channel="ibm_cloud", token="<api-token>", instance="<IBM Cloud CRN>")

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()

# executes three Bell circuits
with Sampler(circuits=[bell]*3, service=service, options={ "backend": "ibmq_qasm_simulator" }) as sampler:
    result = sampler(circuit_indices=[0, 1, 2])
    print(result)
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{'11': 0.5009765625, '00': 0.4990234375}, {'11': 0.5224609375, '00': 0.4775390625}, {'11': 0.4921875, '00': 0.5078125}], metadata=[{'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}])
SamplerResult(quasi_dists=[{'00': 0.5185546875, '11': 0.4814453125}, {'11': 0.498046875, '00': 0.501953125}], metadata=[{'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}])
SamplerResult(quasi_dists=[{'00': 0.458984375, '11': 0.541015625}, {'11': 0.49609375, '00': 0.50390625}], metadata=[{'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}])
```


## Multiple parameterized circuits example
{: #mult-parm-example-sampler}

In this example, we run multiple parameterized circuits. When it is run, this line `result = sampler(circuit_indices=[0, 0, 1], parameter_values=[theta1, theta2, theta3])` specifies which parameter to send to each circuit.  

In our example, the parameter labeled `theta` is sent to the first circuit, `theta2` is sent to the first circuit, and `theta3` is sent to the second circuit.

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler
from qiskit import QuantumCircuit
from qiskit.circuit.library import RealAmplitudes

service = QiskitRuntimeService(channel="ibm_cloud", token="<api-token>", instance="<IBM Cloud CRN>")

# parameterized circuit
pqc = RealAmplitudes(num_qubits=2, reps=2)
pqc.measure_all()
pqc2 = RealAmplitudes(num_qubits=2, reps=3)
pqc2.measure_all()

theta1 = [0, 1, 1, 2, 3, 5]
theta2 = [1, 2, 3, 4, 5, 6]
theta3 = [0, 1, 2, 3, 4, 5, 6, 7]

with Sampler(circuits=[pqc, pqc2], service=service, options={ "backend": "ibmq_qasm_simulator" }) as sampler:
    result = sampler(circuit_indices=[0, 0, 1], parameter_values=[theta1, theta2, theta3])
    print(result)
```
{: codeblock}

### Result
{: #example3-result-sampler}

The results align with the parameter - circuit pairs specified previously.  For example, the first result (`{'11': 0.42578125, '00': 0.14453125, '10': 0.0888671875, '01': 0.3408203125}`) is the output of the parameter labeled `theta` being sent to the first circuit.

```text
SamplerResult(quasi_dists=[{'11': 0.42578125, '00': 0.14453125, '10': 0.0888671875, '01': 0.3408203125}, {'01': 0.025390625, '11': 0.3046875, '00': 0.0615234375, '10': 0.6083984375}, {'11': 0.0224609375, '00': 0.171875, '10': 0.095703125, '01': 0.7099609375}], metadata=[{'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}, {'header_metadata': None, 'shots': 1024}])
```

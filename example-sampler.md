---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-14"

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

With the Sampler primitive you can more accurately contextualize counts. It takes a circuit as input and generates quasiprobabilities. This enables you to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.  

If you are using the Standard plan, running Sampler incurs a cost. See [Manage costs](/docs/quantum-computing?topic=quantum-computing-cost) for cost information.
{: note}

## Prepare the environment
{: #example-sampler-byb}
{: step}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-get-started) to get your Quantum Service instance ready to use.
2. You need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

## Start a session
{: #start-session-sampler-example}
{: step}

With Qiskit Runtime primitives, we introduce the concept of a session that allows you to define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data you send so it doesn't have to be transmitted to the quantum data center on each iteration. You can create a Runtime session by using the context manager (`with ...:`), which automatically opens and closes the session for you.

When you start the session, you can specify these values:

*  **backend**: The system or simulator to run on. If none is specified, the least busy backend is used.
*  **max_time**: How long a runtime session can be open before it is forcibly closed. Can be specified as seconds (int) or a string like "2h 30m 40s". This value must be between 300 seconds and the system imposed maximum time. See [this FAQ](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/faqs/max_execution_time.html){: external} for details.

This is an experimental setting and can break between releases without warning.
{: Note}

See the [sessions](/docs/quantum-computing?topic=quantum-computing-sessions) topic for more information.

## Create a Sampler instance
{: #rw-session-sampler-example}
{: step}

You can make one or more calls to the Sampler primitive within a session, by first creating a Sampler instance.

The `Sampler` class takes in an options variable to control the execution environment. Options can be either a dictionary or a `qiskit_ibm_runtime.Options` class instance. Initializing it as an `Options` class allows you to use auto complete.

There are many settings you can specify as options.  These are some commonly used options:

* **optimization_level**: How much optimization to perform on the circuits. Higher levels generate more optimized circuits, at the expense of longer transpilation times. Values are an integer from 0 (no optimization) to 3 (heaviest optimization). The default value is 1 (light optimization). The default is 1.
* **resilience_level**: How much resilience to build against errors. Higher levels generate more accurate results, at the expense of longer processing times. The default is 0.

For all options, see the [Options API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.options.Options.html#qiskit_ibm_runtime.options.Options){: external}.

Example:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Options, Sampler

service = QiskitRuntimeService()
options = Options(optimization_level=1)

with Session(service=service, backend="ibmq_qasm_simulator"):
    sampler = Sampler(options=options)
```
{: codeblock}

## Invoke the Sampler primitive
{: #sampler-invoke}
{: step}

Use the Sampler instance's `run()` method to launch the Sampler primitive program. The method returns an `IBMRuntimeJob` instance that you can use to query for information such as job ID and status. The job's `result()` method returns the result.

You can invoke the `run()` method multiple times with the different inputs within a session.

### Specify program inputs
{: #estimator-spec-input}
{: step}

The `Sampler.run()` method takes in the following arguments:

* A list of **circuits** that you want to investigate.
* Optional: An optional list of concrete **parameter_values** to be bound.
* Optional: Extra options (**kwargs**) to overwrite the default values.

For more information, see the [Sampler API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Sampler.html){: external}.

Example:

```Python
# Prepare the input circuit.

from qiskit import QuantumCircuit

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()
```
{: codeblock}


### Run the job and print results
{: #sampler-run}
{: step}

Run the job, specifying your previously defined inputs and options. In each call, you specify which circuits to run and, if applicable, `parameter_values` to specify which parameter to use with each circuit. This example has one circuit and it doesn't have parameters.

```Python
# Execute the Bell circuit
with Session(service=service, backend="ibmq_qasm_simulator"):
    sampler = Sampler(options=options)
    job = sampler.run(circuits=bell)
    print(job.result())

    # You can invoke run() multiple times.
    job = sampler.run(circuits=bell)
    print(job.result())
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{3: 0.494, 0: 0.506}], metadata=[{'header_metadata': {}, 'shots': 4000}])
SamplerResult(quasi_dists=[{0: 0.4985, 3: 0.5015}], metadata=[{'header_metadata': {}, 'shots': 4000}])
```

## Example with multiple circuits
{: #example2-sampler}

In this example, we specify three circuits, but they have no parameters:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Options, Sampler
from qiskit import QuantumCircuit

service = QiskitRuntimeService()
options = Options(optimization_level=1)

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()

# executes three Bell circuits
with Session(service=service, backend="ibmq_qasm_simulator"):
    sampler = Sampler(options=options)
    job = sampler.run(circuits=[bell]*3)
    print(job.result())
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{0: 0.49025, 3: 0.50975}, {3: 0.501, 0: 0.499}, {0: 0.4825, 3: 0.5175}], metadata=[{'header_metadata': {}, 'shots': 4000}, {'header_metadata': {}, 'shots': 4000}, {'header_metadata': {}, 'shots': 4000}])
```


## Multiple parameterized circuits example
{: #mult-parm-example-sampler}

In this example, we run multiple parameterized circuits. When it is run, this line `result = sampler(circuits=[0, 0, 1], parameter_values=[theta1, theta2, theta3])` specifies which parameter to send to each circuit.

In our example, parameter `theta1` is sent to the first circuit, `theta2` is sent to the first circuit, and `theta3` is sent to the second circuit.

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Options, Sampler
from qiskit import QuantumCircuit
from qiskit.circuit.library import RealAmplitudes

service = QiskitRuntimeService()
options = Options(optimization_level=1)

# parameterized circuit
pqc = RealAmplitudes(num_qubits=2, reps=2)
pqc.measure_all()
pqc2 = RealAmplitudes(num_qubits=2, reps=3)
pqc2.measure_all()

theta1 = [0, 1, 1, 2, 3, 5]
theta2 = [1, 2, 3, 4, 5, 6]
theta3 = [0, 1, 2, 3, 4, 5, 6, 7]

with Session(service=service, backend="ibmq_qasm_simulator"):
    sampler = Sampler(options=options)

    job = sampler.run(circuits=[pqc, pqc, pqc2], parameter_values=[theta1, theta2, theta3])
    print(job.result())
```
{: codeblock}

### Result
{: #example3-result-sampler}

The results align with the parameter - circuit pairs that were previously specified. For example, the first result `({2: 0.09375, 0: 0.12975, 3: 0.41875, 1: 0.35775})` is the output of the `theta1` parameter being sent to the first circuit.

```text
SamplerResult(quasi_dists=[{2: 0.09375, 0: 0.12975, 3: 0.41875, 1: 0.35775}, {1: 0.033, 0: 0.05975, 2: 0.61275, 3: 0.2945}, {0: 0.1845, 2: 0.1055, 3: 0.03325, 1: 0.67675}], metadata=[{'header_metadata': {}, 'shots': 4000}, {'header_metadata': {}, 'shots': 4000}, {'header_metadata': {}, 'shots': 4000}])
```

---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-05"

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

If you are using the Standard plan, running Sampler incurs a cost. See [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost) for cost information.
{: note}

## Prepare the environment
{: #example-sampler-byb}
{: step}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your Quantum Service instance ready to use.
2. You need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

## Start a session
{: #start-session-estimator-example}
{: step}

With Qiskit Runtime primitives, we introduce sessions. With sessions, you can define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data that you send so it isn't transmitted to the quantum data center on each iteration. See the [sessions](/docs/quantum-computing?topic=quantum-computing-sessions) topic for more information.

### Create a Sampler instance
{: #sampler-inputs}
{: step}

The Sampler class takes in:
* A **session** object that represents the sessions to use.
* Optional: **options** as a dictionary or a **qiskit_ibm_runtime.Options** class instance. Initializing it as a class instance enables auto-complete. Some available options include the following. For the full list, see the [Options API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Options.html){: external}.
    *  backend: The string name of a backend to run on. If none is specified, the least busy backend is used. To learn about choosing a backend, see [Choose a system or simulator](/docs/quantum-computing?topic=quantum-computing-choose-backend).
    *  log_level: The logging level to set in the execution environment. The valid log levels are: `DEBUG`, `INFO`, `WARNING`, `ERROR`, and `CRITICAL`. The default level is `WARNING`.
    *  optimization_level: How much optimization to apply to the circuits. Higher levels generate more optimized circuits, at the expense of longer transpilation times. Values are an integer from 0 (no optimization) to 3 (heaviest optimization). The default value is 1 (light optimization).
    *  resilience_level: How much resilience to build against errors. Higher levels generate more accurate results, at the expense of longer processing times.

Example:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Options, Sampler
from qiskit import QuantumCircuit

service = QiskitRuntimeService()
options = Options(backend="ibmq_qasm_simulator", optimization_level=1)

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()

with Session(service=service) as session:
    sampler = Sampler(session=session, options=options)
```
{: codeblock}

## Invoke the Sampler primitive
{: #sampler-invoke}
{: step}

Use the run() method of the Sampler instance to launch the Sampler primitive program. The method returns an IBMRuntimeJob instance that you can use to query for information such as job ID and status. The job's result() method returns the result.

You can invoke the run() method multiple times with the different inputs within a session.

### Specify program inputs
{: #estimator-spec-input}
{: step}

The Sampler.run() method takes in the following arguments:

* A list of **circuits** that you want to investigate.
* Optional: A list of **parameters** for any parameterized circuits.
* Optional: An optional list of concrete **parameter_values** to be bound.
* Optional: Extra options (**kwargs**) to overwrite the default values.

For more information, see the [Sampler API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Sampler.html){: external}.


### Run the job and print results
{: #sampler-run}
{: step}

Run the job, specifying your previously defined inputs and options. In each call, you specify which circuits to run and, if applicable, `parameter_values` specifies which parameter to use with each circuit. This example has one circuit and it doesn't have parameters.

```Python
# executes a Bell circuit
with Session(service=service) as session:
    sampler = Sampler(session=session, options=options)
    job = sampler.run(circuits=bell)
    print(job.result())

    # You can invoke run() multiple times.
    job = sampler.run(circuits=bell)
    print(job.result())
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{'11': 0.486328125, '00': 0.513671875}], metadata=[{'header_metadata': {}, 'shots': 1024}])
```

## Example with multiple circuits
{: #example2-sampler}

In this example, we specify three circuits, but they have no parameters:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Options, Sampler
from qiskit import QuantumCircuit

service = QiskitRuntimeService()
options = Options(backend="ibmq_qasm_simulator", optimization_level=1)

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()

# executes three Bell circuits
with Session(service=service) as session:
    sampler = Sampler(session=session, options=options)
    job = sampler.run(circuits=[bell]*3)
    print(job.result())
```
{: codeblock}

```text
SamplerResult(quasi_dists=[{'11': 0.484375, '00': 0.515625}, {'00': 0.5029296875, '11': 0.4970703125}, {'00': 0.4794921875, '11': 0.5205078125}], metadata=[{'header_metadata': {}, 'shots': 1024}, {'header_metadata': {}, 'shots': 1024}, {'header_metadata': {}, 'shots': 1024}])
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
options = Options(backend="ibmq_qasm_simulator", optimization_level=1)

# parameterized circuit
pqc = RealAmplitudes(num_qubits=2, reps=2)
pqc.measure_all()
pqc2 = RealAmplitudes(num_qubits=2, reps=3)
pqc2.measure_all()

theta1 = [0, 1, 1, 2, 3, 5]
theta2 = [1, 2, 3, 4, 5, 6]
theta3 = [0, 1, 2, 3, 4, 5, 6, 7]

with Session(service) as session:
    sampler = Sampler(session=session, options=options)

    job = sampler.run(circuits=[pqc, pqc, pqc2], parameter_values=[theta1, theta2, theta3])
    print(job.result())
```
{: codeblock}

### Result
{: #example3-result-sampler}

The results align with the parameter - circuit pairs that were previously specified. For example, the first result `({'00': 0.1376953125, '10': 0.095703125, '01': 0.359375, '11': 0.4072265625})` is the output of the `theta1` parameter being sent to the first circuit.

```text
SamplerResult(quasi_dists=[{'01': 0.3828125, '11': 0.4150390625, '00': 0.1171875, '10': 0.0849609375}, {'01': 0.0419921875, '11': 0.3095703125, '00': 0.0751953125, '10': 0.5732421875}, {'01': 0.7080078125, '11': 0.03515625, '10': 0.08203125, '00': 0.1748046875}], metadata=[{'header_metadata': {}, 'shots': 1024}, {'header_metadata': {}, 'shots': 1024}, {'header_metadata': {}, 'shots': 1024}])
```

---

copyright:
  years: 2021, 2023
lastupdated: "2022-10-14"

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

Use the examples in this topic to learn how to set up and use the Estimator primitive program.
{: shortdesc}

## Overview
{: #estimator-overview}

With the Estimator primitive, you can efficiently calculate and interpret expectation values of quantum operators that are required for many algorithms. You can specify a list of circuits and observables, then evaluate expectation values and variances for an input parameter.

If you are using physical backends, running Estimator incurs a cost. For cost details, see [Manage costs](/docs/quantum-computing?topic=quantum-computing-cost).
{: note}

## Prepare the environment
{: #example-estimator-byb}

Before running any of these examples, you must prepare your environment. 

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-get-started) to get your quantum service instance ready to use.

2. You need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

3. Create a list of observables. Use observables to define the properties of the circuit that are relevant to your problem and  efficiently measure their expectation value. For simplicity, you can use the [PauliSumOp class](https://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.html#module-qiskit.opflow.primitive_ops){: external} in Qiskit to define them, as illustrated in the following example.


## Single experiment example
{: #estimator-inputs}

```Python
from qiskit.circuit.random import random_circuit 
from qiskit.quantum_info import SparsePauliOp 
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the next line if you want to run on a system
# backend = service.least_busy(simulator=False)

circuit = random_circuit(2, 2, seed=1234) 
observable = SparsePauliOp("IY") 

estimator = Estimator(backend) 
job = estimator.run(circuit, observable) 
result = job.result() 

print(circuit) 
print(f" > Observable: {observable.paulis}") 
print(f" > Expectation value: {result.values}") 
print(f" > Metadata: {result.metadata}")
```
{: codeblock}

For more information, see the [Estimator API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Estimator.html){: external}.
## Multiple circuit example
{: #example2-estimator}

Use Estimator to determine the expectation values of multiple circuit-observable pairs.

```python 
from qiskit.circuit.random import random_circuit 
from qiskit.quantum_info import SparsePauliOp 
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system
# backend = service.least_busy(simulator=False)

circuits = [random_circuit(2, 2, seed=i) for i in range(4)] 
observables = [ 
    SparsePauliOp("IY"), 
    SparsePauliOp("XY"), 
    SparsePauliOp("ZI"), 
    SparsePauliOp("ZX"), 
] 

estimator = Estimator(backend) 
job = estimator.run(circuits, observables) 
result = job.result() 

print(f" > Expectation values: {result.values}")
```
{: codeblock}

## Multiple parameterized circuits example
{: #mult-parm-example-estimator}

Use Estimator to run three experiments in a single job, leveraging parameter values to increase circuit reusability. This line `job = estimator.run([circuit] * 3, parameter_values)` specifies which parameter to send to each circuit.

```python 
from qiskit.circuit.library import RealAmplitudes
from qiskit.quantum_info import SparsePauliOp
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator

service = QiskitRuntimeService()

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

circuit = RealAmplitudes(num_qubits=2, reps=2)
# Define three sets of parameters for the circuit
parameter_values = [
    [0, 1, 2, 3, 4, 5],
    [1, 1, 2, 3, 5, 8],
    [0, 0.1, 0.2, 0.3, 0.4, 0.5],
]
observable = SparsePauliOp("ZI")

estimator = Estimator(backend)
job = estimator.run([circuit] * 3, [observable] * 3, parameter_values)
result = job.result()

print(f" > Expectation values: {result.values}")
```
{: codeblock}

## Sessions and advanced options example
{: #start-session-estimator-example}

Explore sessions and advanced options to optimize circuit performance on quantum systems.  With Qiskit Runtime primitives, we introduce the concept of a session that allows you to define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data you send so it doesn't have to be transmitted to the quantum data center on each iteration. You can create a Runtime session by using the context manager (`with ...:`), which automatically opens and closes the session for you. See [Sessions](/docs/quantum-computing?topic=quantum-computing-sessions) for details.

When you start the session, you can specify these values:

*  **backend**: The system or simulator to run on. If none is specified, the least busy backend is used.
*  **max_time**: How long a runtime session can be open before it is forcibly closed. Can be specified as seconds (int) or a string like `"2h 30m 40s"`. This value must be between 300 seconds and the system imposed maximum time. See [Maximum execution time for a Qiskit Runtime job](https://qiskit.org/ecosystem/ibm-runtime/faqs/max_execution_time.html){: external} for details.

This is an experimental setting and can break between releases without warning.
{: Note}

Explore sessions and advanced options to optimize circuit performance on quantum systems.

```python 
from qiskit.circuit.random import random_circuit 
from qiskit.quantum_info import SparsePauliOp 
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Estimator, Options 

circuit = random_circuit(2, 2, seed=1) 
another_circuit = random_circuit(3, 3, seed=1) 
observable = SparsePauliOp("IY") 
another_observable = SparsePauliOp("XYZ") 

options = Options() 
options.optimization_level = 2 
options.resilience_level = 2 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

with Session(service=service, backend=backend) as session: 
    estimator = Estimator(session=session, options=options) 
    job = estimator.run(circuit, observable) 
    another_job = estimator.run(another_circuit, another_observable) 
    result = job.result() 
    another_result = another_job.result() 

# first job 
print(f" > Expectation values job 1: {result.values}") 

# second job 
print(f" > Expectation values job 2: {another_result.values}")
```

{: codeblock}
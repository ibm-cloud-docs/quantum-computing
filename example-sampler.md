---

copyright:
  years: 2021, 2023
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

Use the examples in this topic to learn how to set up and use the Sampler primitive program.
{: shortdesc}

## Overview
{: #sampler-overview}

With the Sampler primitive you can more accurately contextualize counts. It takes a circuit as input and generates quasiprobabilities. This enables you to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference.  

If you are using the Standard plan, running Sampler incurs a cost. See [Manage costs](/docs/quantum-computing?topic=quantum-computing-cost) for cost information.
{: note}

## Prepare the environment
{: #example-sampler-byb}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-get-started) to get your Quantum Service instance ready to use.
2. You need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

## Single experiment example
{: #rw-session-sampler-example}

```Python
from qiskit.circuit.random import random_circuit 
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

circuit = random_circuit(2, 2, seed=1234) 
circuit.measure_all() 

sampler = Sampler(backend) 
job = sampler.run(circuit) 
result = job.result() 

print(circuit) 
print(f" > Quasiprobability distribution: {result.quasi_dists}") 
print(f" > Metadata: {result.metadata}") 
```
{: codeblock}

For more information, see the [Sampler API reference](https://docs.quantum-computing.ibm.com/api/qiskit-ibm-runtime/qiskit_ibm_runtime.Sampler){: external}.

## Multiple circuit example
{: #example2-sampler}

Use Sampler to determine the quasiprobability distributions of multiple circuits in one job.

```python
from qiskit.circuit.random import random_circuit 
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler 

service = QiskitRuntimeService()

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

circuits = [random_circuit(2, 2, measure=True, seed=i) for i in range(4)] 

sampler = Sampler(backend) 
job = sampler.run(circuits) 
result = job.result() 

print(f" > Quasiprobability distribution: {result.quasi_dists}")
```
{: codeblock}

## Multiple parameterized circuits example
{: #mult-parm-example-sampler}

Run three experiments in a single job, leveraging parameter values to increase circuit reusability. This line `job = sampler.run([circuit] * 3, parameter_values)` specifies which parameter to send to each circuit.

```python
from qiskit.circuit.library import RealAmplitudes 
from qiskit_ibm_runtime import QiskitRuntimeService, Sampler 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

circuit = RealAmplitudes(num_qubits=2, reps=2) 
circuit.measure_all() 
# Define three sets of parameters for the circuit 
parameter_values = [ 
    [0, 1, 2, 3, 4, 5], 
    [1, 1, 2, 3, 5, 8], 
    [0, 0.1, 0.2, 0.3, 0.4, 0.5], 
] 

sampler = Sampler(backend) 
job = sampler.run([circuit] * 3, parameter_values) 
result = job.result() 

print(f" > Quasiprobability distribution: {result.quasi_dists}")
```
{: codeblock}

## Sessions and advanced options example
{: #start-session-sampler-example}

Explore sessions and advanced options to optimize circuit performance on quantum systems.  With Qiskit Runtime primitives, we introduce the concept of a session that allows you to define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data you send so it doesn't have to be transmitted to the quantum data center on each iteration. You can create a Runtime session by using the context manager (`with ...:`), which automatically opens and closes the session for you. See [Sessions](/docs/quantum-computing?topic=quantum-computing-sessions) for details.

When you start the session, you can specify these values:

*  **backend**: The system or simulator to run on. If none is specified, the least busy backend is used.
*  **max_time**: How long a runtime session can be open before it is forcibly closed. Can be specified as seconds (int) or a string like "2h 30m 40s". This value must be between 300 seconds and the system imposed maximum time. See [Maximum execution time for a Qiskit Runtime job](https://qiskit.org/ecosystem/ibm-runtime/faqs/max_execution_time.html){: external} for details.

This is an experimental setting and can break between releases without warning.
{: Note}

```python
from qiskit.circuit.random import random_circuit 
from qiskit.quantum_info import SparsePauliOp 
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Sampler, Options 

circuit = random_circuit(2, 2, measure=True, seed=1) 
another_circuit = random_circuit(3, 3, measure=True, seed=1) 

options = Options() 
options.optimization_level = 2 
options.resilience_level = 0 

service = QiskitRuntimeService() 

# Run on a simulator
backend = service.get_backend("ibmq_qasm_simulator")
# Use the following line if you want to run on a system instead of a simulator:
# backend = service.least_busy(simulator=False)

with Session(service=service, backend=backend) as session: 
    sampler = Sampler(session=session, options=options) 
    job = sampler.run(circuit) 
    another_job = sampler.run(another_circuit) 
    result = job.result()
    another_result = another_job.result() 

# first job 
print(f" > Quasiprobability distribution job 1: {result.quasi_dists}") 

# second job 
print(f" > Quasiprobability distribution job 2: {another_result.quasi_dists}")
```
{: codeblock}

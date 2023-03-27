---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-22"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, IBM Quantum Platform

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Calculate expectation values in an algorithm
{: #migrate-estimator}

The Estimator primitive is used to design an algorithm that calculates
expectation values.
{: #shortdesc}

## Background
{: #migrate-est-bg}

The role of the `Estimator` primitive is two-fold: it acts as an **entry point** to quantum devices or simulators, replacing the `Backend`
interface (commonly referred to as `backend.run()`). Additionally, it is
an **algorithmic abstraction** for expectation value calculations, so
you don't have to manually construct the final expectation circuit.
This results in a considerable reduction of the code complexity and a
more compact algorithm design.

**Backend.run() model:** In this model, you accessed real backends and
remote simulators using the `qiskit-ibmq-provider` module (now migrated
to `qiskit-ibm-provider`). To run **local** simulations, you could
import a specific backend from `qiskit-aer`. All of them followed the
`backend.run()` interface.
{: note}

<details>
<summary>Code example for qiskit-ibmq-provider & backend.run()</summary>

``` python
from qiskit import IBMQ

# Select provider
provider = IBMQ.get_provider(hub="ibm-q", group="open", project="main")

# Get backend
backend = provider.get_backend("ibmq_qasm_simulator") # cloud simulator

# Run
result = backend.run(expectation_circuits)
```
{: codeblock}

</details>

<details>
<summary>Code example for qiskit-aer & backend.run()</summary>

``` python
from qiskit_aer import AerSimulator # former import: from qiskit import Aer

# Get local simulator backend
backend = AerSimulator()

# Run
result = backend.run(expectation_circuits)
```
{: codeblock}

</details>

**Primitives model:** Access real backends and remote simulators through
the `qiskit-ibm-runtime` **primitives** (`Sampler` and `Estimator`). To
run **local** simulations, you can import specific local
primitives from `qiskit_aer.primitives` and `qiskit.primitives`. All
of them follow the `BaseSampler` and `BaseEstimator` interfaces, but
**only the Runtime primitives offer access to the Runtime service, sessions, and built-in error mitigation**.

<details>
<summary>Code example for Runtime Estimator</summary>

``` python
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator

# Define service
service = QiskitRuntimeService()

# Get backend
backend = service.backend("ibmq_qasm_simulator") # cloud simulator

# Define Estimator
# (see tutorials more more info on sessions)
estimator = Estimator(session=backend)

# Run Expectation value calculation
result = estimator.run(circuits, observables).result()
```
{: codeblock}

</details>

<details>
<summary>Code example for Aer Estimator</summary>

``` python
from qiskit_aer import Estimator

# Get local simulator Estimator
estimator = Estimator()

# Run expectation value calculation
result = estimator.run(circuits, observables).result()
```
{: codeblock}

</details>

If your code previously calculated expectation values using
`backend.run()`, you most likely used the `qiskit.opflow` module to
handle operators and state functions. To support this scenario, the
following migration example shows how to replace the (`qiskit.opflow`
& `backend.run()`) workflow with an `Estimator`-based workflow.

## End-to-end example
{: #mig-est-e2e}

## 1. Problem definition
{: #mig-est-prob-def}

We want to compute the expectation value of a quantum state (circuit)
with respect to a certain operator. In this example, we are using the H2
molecule and an arbitrary circuit as the quantum state:

``` python
from qiskit import QuantumCircuit
from qiskit.quantum_info import SparsePauliOp

# Step 1: Define operator
op = SparsePauliOp.from_list(
    [
        ("II", -1.052373245772859),
        ("IZ", 0.39793742484318045),
        ("ZI", -0.39793742484318045),
        ("ZZ", -0.01128010425623538),
        ("XX", 0.18093119978423156),
    ]
)

# Step 2: Define quantum state
state = QuantumCircuit(2)
state.x(0)
state.x(1)
```
{: codeblock}

### 1.a. Legacy: Convert problem to `opflow`
{: #mig-est-legacy}

`qiskit.opflow` provided its own classes to represent both operators
and quantum states, so the problem defined above would be wrapped as:

``` python
from qiskit.opflow import CircuitStateFn, PauliSumOp

opflow_op = PauliSumOp(op)
opflow_state = CircuitStateFn(state)
```
{: codeblock}

This step is no longer necessary when using the primitives.

## 2. Calculate expectation values on real device or cloud simulator
{: #mig-est2}

### 2.a. Legacy: Use `opflow` & `backend.run()`
{: #mig-est2a}

The legacy workflow required many steps to compute an expectation value:

Replace `ibmq_qasm_simulator` with your device name to see the complete
workflow for a real device.
{: note}

``` python
from qiskit.opflow import StateFn, PauliExpectation, CircuitSampler
from qiskit import IBMQ

# Define the state to sample
measurable_expression = StateFn(opflow_op, is_measurement=True).compose(opflow_state)

# Convert to expectation value calculation object
expectation = PauliExpectation().convert(measurable_expression)

# Define provider and backend
provider = IBMQ.get_provider(hub="ibm-q", group="open", project="main")
backend = provider.get_backend("ibmq_qasm_simulator")

# Inject backend into circuit sampler
sampler = CircuitSampler(backend).convert(expectation)

# Evaluate
expectation_value = sampler.eval().real
```
{: codeblock}

``` python
>>> print("expectation: ", expectation_value)
expectation:  -1.065734058826613
```
{: codeblock}

### 2.b. New: Use the `Estimator` Runtime primitive
{: #mig-est2b}

The `Estimator` simplifies the user-side syntax, making it a more
convenient tool for algorithm design.

Replace `ibmq_qasm_simulator` with your device name to see the complete
workflow for a real device.
{: note}

``` python
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator

service = QiskitRuntimeService(channel="ibm_quantum")
backend = service.backend("ibmq_qasm_simulator")

estimator = Estimator(session=backend)

expectation_value = estimator.run(state, op).result().values
```
{: codeblock}

Note that the Estimator returns a list of values, as it can perform
batched evaluations.

``` python
>>> print("expectation: ", expectation_value)
expectation:  [-1.06329149]
```
{: codeblock}

The `Estimator` Runtime primitive offers a series of features and tuning
options that do not have a legacy alternative to migrate from, but can
help improve your performance and results. For more information, refer
to the following:

- [Error mitigation tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/Error-Suppression-and-Error-Mitigation.html){: external}
- [Setting execution options topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/how_to/options.html){: external}
- [Primitive execution options API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.options.Options.html#qiskit_ibm_runtime.options.Options){: external}
- [How to run a session topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/how_to/run_session.html){: external}

## 3. Other execution alternatives (non-Runtime)
{: #mig-est-3}

This section describes how to use non-Runtime primitives to test an
algorithm using local simulation. Let's assume that we want to solve
the problem defined above with a local statevector simulation.

### 3.a. Legacy: Use the Qiskit Aer simulator
{: #mig-est-3a}

``` python
from qiskit.opflow import StateFn, PauliExpectation, CircuitSampler
from qiskit_aer import AerSimulator

# Define the state to sample
measurable_expression = StateFn(opflow_op, is_measurement=True).compose(opflow_state)

# Convert to expectation value calculation object
expectation = PauliExpectation().convert(measurable_expression)

# Define statevector simulator
simulator = AerSimulator(method="statevector", shots=100)

# Inject backend into circuit sampler
circuit_sampler = CircuitSampler(simulator).convert(expectation)

# Evaluate
expectation_value = circuit_sampler.eval().real
```
{: codeblock}

``` python
>>> print("expectation: ", expectation_value)
expectation:  -1.0636533500290943
```
{: codeblock}

### 3.b. New: Use the Reference `Estimator` or Aer `Estimator` primitive
{: #mig-est-3b}

The Reference `Estimator` lets you perform either an exact or a
shot-based noisy simulation based on the `Statevector` class in the
`qiskit.quantum_info` module.

``` python
from qiskit.primitives import Estimator

estimator = Estimator()

expectation_value = estimator.run(state, op).result().values

# for shot-based simulation:
expectation_value = estimator.run(state, op, shots=100).result().values
```
{: codeblock}

``` python
>>> print("expectation: ", expectation_value)
expectation:  [-1.03134297]
```
{: codeblock}

You can still access the Aer Simulator through its dedicated
`Estimator`. This can be useful for performing simulations with noise
models. In this example, the simulation method has been updated to match
the result from 3.a:

``` python
from qiskit_aer.primitives import Estimator # import change!!!

estimator = Estimator(run_options= {"method": "statevector"})

expectation_value = estimator.run(state, op, shots=100).result().values
```
{: codeblock}

``` python
>>> print("expectation: ", expectation_value)
expectation:  [-1.06365335]
```
{: codeblock}

For more information on using the Aer primitives, see the [VQE tutorial](https://qiskit.org/documentation/tutorials/algorithms/03_vqe_simulation_with_noise.html){: external}.

For more information about running noisy simulations with the **Runtime primitives**, see [Noisy Simulators in Qiskit Runtime](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/how_to/noisy_simulators.html){: external}.


---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-07"


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

If you are using the Standard plan, running Estimator incurs a cost. See [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost) for cost information.
{: note}

## Prepare the environment
{: #example-estimator-byb}
{: step}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your quantum service instance ready to use.

2. You'll need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

3. Create a list of observables. Observables let you define the properties of the circuit that are relevant to your problem and enable you to efficiently measure their expectation value. For simplicity, you can use the [PauliSumOp class](https://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.html#module-qiskit.opflow.primitive_ops){: external} in Qiskit to define them, as illustrated in the example below.

## Start a session
{: #start-session-estimator-example}
{: step}

With Qiskit Runtime primitives, we introduce the concept of a session that allows you to define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data you send so it doesn't have to be transmitted to the quantum data center on each iteration. See the [sessions](/docs/quantum-computing?topic=quantum-computing-sessions) topic for more information.

### Specify program inputs
{: #estimator-inputs}
{: step}

The Estimator takes in:
* The **circuits** you want to investigate.
* The **service** to use.
* The **observables** (Hamiltonians) to be evaluated.
* Optional: **options**, such as the backend to run on. If a backend is not specified, the least busy backend is used. To learn about choosing a backend, see [Choose a system or simulator](/docs/quantum-computing?topic=quantum-computing-choose-backend).
* Optional: The instruction to **skip_transpilation**.

Example:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Estimator
from qiskit.circuit.library import RealAmplitudes
from qiskit.quantum_info import SparsePauliOp

service = QiskitRuntimeService(channel="ibm_cloud", token="<api-token>", instance="<IBM Cloud CRN>")

psi1 = RealAmplitudes(num_qubits=2, reps=2)
psi2 = RealAmplitudes(num_qubits=2, reps=3)

H1 = SparsePauliOp.from_list([("II", 1), ("IZ", 2), ("XI", 3)])
H2 = SparsePauliOp.from_list([("IZ", 1)])
H3 = SparsePauliOp.from_list([("ZI", 1), ("ZZ", 1)])
```
{: codeblock}

### Run the job & print results
{: #estimator-run}
{: step}

Run the job, specifying your previously defined inputs and options.  Use `circuit_indices`, `observable_indices`, and `parameter_values` to use a specific parameter and observable with the specified circuit.

For example, this line `psi1_H23_result = estimator(circuit_indices=[0, 0], observable_indices=[1, 2], parameter_values=[theta1]*2)` specifies the following:

- Return the value for using observable `H2` and parameter `theta1` with the circuit `psi1`.
- Return the value for using observable `H3` and parameter `theta1` with the circuit `psi1`.


```Python
with Estimator(
    circuits=[psi1, psi2],
    observables=[H1, H2, H3],
    service=service,
    options={ "backend": "ibmq_qasm_simulator" }
) as estimator:
    theta1 = [0, 1, 1, 2, 3, 5]
    theta2 = [0, 1, 1, 2, 3, 5, 8, 13]
    theta3 = [1, 2, 3, 4, 5, 6]

    # calculate [ <psi1(theta1)|H1|psi1(theta1)> ]
    psi1_H1_result = estimator(circuit_indices=[0], observable_indices=[0], parameter_values=[theta1])
    print(psi1_H1_result)

    # calculate [ <psi1(theta1)|H2|psi1(theta1)>, <psi1(theta1)|H3|psi1(theta1)> ]
    psi1_H23_result = estimator(circuit_indices=[0, 0], observable_indices=[1, 2], parameter_values=[theta1]*2)
    print(psi1_H23_result)

    # calculate [ <psi2(theta2)|H2|psi2(theta2)> ]
    # Note that you don't need to specify the labels "circuit_indices", "observable_indices", or "parameter_values", as long as they are specified in that order.
    psi2_H2_result = estimator([1], [1], [theta2])
    print(psi2_H2_result)

    # calculate [ <psi1(theta1)|H1|psi1(theta1)>, <psi1(theta3)|H1|psi1(theta3)> ]
    psi1_H1_result2 = estimator([0, 0], [0, 0], [theta1, theta3])
    print(psi1_H1_result2)

    # calculate [ <psi1(theta1)|H1|psi1(theta1)>,
    #             <psi2(theta2)|H2|psi2(theta2)>,
    #             <psi1(theta3)|H3|psi1(theta3)> ]
    psi12_H23_result = estimator([0, 1, 0], [0, 1, 2], [theta1, theta2, theta3])
    print(psi12_H23_result)
```
{: codeblock}

The results align with the parameter - circuit - observable tuples specified previously.  For example, the first result: `EstimatorResult(values=array([1.55273438]), metadata=[{'variance': 8.897655487060547, 'shots': 1024}])` is the output of the parameter labeled `theta1` and observable `H1` being sent to the first circuit.

Output:
```text
EstimatorResult(values=array([1.55273438]), metadata=[{'variance': 8.897655487060547, 'shots': 1024}])
EstimatorResult(values=array([-0.55664062, 0.07421875]), metadata=[{'variance': 0.6901512145996094, 'shots': 1024}, {'variance': 1.9938812255859375, 'shots': 1024}])
EstimatorResult(values=array([0.19921875]), metadata=[{'variance': 0.9603118896484375, 'shots': 1024}])
EstimatorResult(values=array([1.515625 , 0.97460938]), metadata=[{'variance': 9.603851318359375, 'shots': 1024}, {'variance': 11.997127532958984, 'shots': 1024}])
EstimatorResult(values=array([ 1.55078125, 0.13085938, -1.04101562]), metadata=[{'variance': 9.120590209960938, 'shots': 1024}, {'variance': 0.9828758239746094, 'shots': 1024}, {'variance': 1.2807121276855469, 'shots': 1024}])
```

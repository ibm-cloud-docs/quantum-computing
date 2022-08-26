---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-26"

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

With the Estimator primitive, you can efficiently calculate and interpret expectation values of quantum operators that are required for many algorithms. You can specify a list of circuits and observables, then evaluate expectation values and variances for an input parameter. 

If you are using the Standard plan, running Estimator incurs a cost. For cost details, see [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost).
{: note}

## Prepare the environment
{: #example-estimator-byb}
{: step}

1. Follow the steps in the [getting started guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to get your quantum service instance ready to use.

2. You need at least one circuit to submit to the program. Our examples all have circuits in them, but if you want to submit your own circuit, you can use Qiskit to create one. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

3. Create a list of observables. Use observables to define the properties of the circuit that are relevant to your problem and  efficiently measure their expectation value. For simplicity, you can use the [PauliSumOp class](https://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.html#module-qiskit.opflow.primitive_ops){: external} in Qiskit to define them, as illustrated in the following example.

## Start a session
{: #start-session-estimator-example}
{: step}

With Qiskit Runtime primitives, we introduce sessions. With sessions, you can define a job as a collection of iterative calls to the quantum computer. When you start a session, it caches the data that you send so it isn't transmitted to the quantum data center on each iteration. See the [sessions](/docs/quantum-computing?topic=quantum-computing-sessions) topic for more information.

### Create an Estimator instance
{: #estimator-inputs}
{: step}

The Estimator class takes in:
* A **session** object that represents the sessions to use.
* Optional: **options** as a dictionary or a **qiskit_ibm_runtime.Options** class instance. Initializing it as a class instance enables auto-complete. Some available options include the following. For the full list, see the [Options API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Options.html){: external}.
    *  backend: The string name of a backend to run on. If none is specified, the least busy backend is used. To learn about choosing a backend, see [Choose a system or simulator](/docs/quantum-computing?topic=quantum-computing-choose-backend).
    *  log_level: The logging level to set in the execution environment. The valid log levels are: `DEBUG`, `INFO`, `WARNING`, `ERROR`, and `CRITICAL`. The default level is `WARNING`.
    *  optimization_level: How much optimization to perform on the circuits. Higher levels generate more optimized circuits, at the expense of longer transpilation times. Values are an integer from 0 (no optimization) to 3 (heaviest optimization). The default value is 1 (light optimization).
    *  resilience_level: How much resilience to build against errors. Higher levels generate more accurate results, at the expense of longer processing times.

Example:

```Python
from qiskit_ibm_runtime import QiskitRuntimeService, Session, Estimator, Options
from qiskit.circuit.library import RealAmplitudes
from qiskit.quantum_info import SparsePauliOp

service = QiskitRuntimeService()
options = Options(backend="ibmq_qasm_simulator", optimization_level=1)

with Session(service) as session:
    estimator = Estimator(session=session, options=options)
```
{: codeblock}

## Invoke the Estimator primitive
{: #estimator-invoke}
{: step}

Use the run() method of the Estimator instance to launch the Estimator primitive program. The method returns an IBMRuntimeJob instance that you can use to query for information such as job ID and status. The job's result() method returns the result.

You can invoke the run() method multiple times with the different inputs within a session.

### Specify program inputs
{: #estimator-spec-input}
{: step}

The Estimator.run() method takes in the following arguments:

* A list of **circuits** that you want to investigate.
* A list of **observables** to measure the expectation values.
* Optional: A list of **parameters** for any parameterized circuits. 
* Optional: An optional list of concrete **parameter_values** to be bound.
* Optional: Extra options (**kwargs**) to overwrite the default values.

For more information, see the [Estimator API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.Estimator.html){: external}.

## Run the job and print results
{: #estimator-run}
{: step}

Run the job, specifying your previously defined inputs and options. Use `circuits`, `observables`, and `parameter_values` to use a specific parameter and observable with the specified circuit.

For example, this line `psi1_H23_result = estimator.run(circuits=[psi1, psi1], observables=[H2, H3], parameter_values=[theta1]*2.result()` specifies the following actions:

- Return the value for using observable `H2` and parameter `theta1` with the circuit `psi1`.
- Return the value for using observable `H3` and parameter `theta1` with the circuit `psi1`.


```Python
with Session(service) as session:
    estimator = Estimator(session=session, options=options)

    theta1 = [0, 1, 1, 2, 3, 5]
    theta2 = [0, 1, 1, 2, 3, 5, 8, 13]
    theta3 = [1, 2, 3, 4, 5, 6]

    # calculate [ <psi1(theta1)|H1|psi1(theta1)> ]
    psi1_H1 = estimator.run(circuits=[psi1], observables=[H1], parameter_values=[theta1])
    print(psi1_H1.result())

    # calculate [ <psi1(theta1)|H2|psi1(theta1)>, <psi1(theta1)|H3|psi1(theta1)> ]
    psi1_H23 = estimator.run(circuits=[psi1, psi1], observables=[H2, H3], parameter_values=[theta1]*2)
    print(psi1_H23.result())

    # calculate [ <psi2(theta2)|H2|psi2(theta2)> ]
    psi2_H2 = estimator.run(circuits=[psi2], observables=[H2], parameter_values=[theta2])
    print(psi2_H2.result())

    # calculate [ <psi1(theta1)|H1|psi1(theta1)>, <psi1(theta3)|H1|psi1(theta3)> ]
    psi1_H1_job = estimator.run(circuits=[psi1, psi1], observables=[H1, H1], parameter_values=[theta1, theta3])
    print(psi1_H1_job.result())

    # calculate [ <psi1(theta1)|H1|psi1(theta1)>,
    #             <psi2(theta2)|H2|psi2(theta2)>,
    #             <psi1(theta3)|H3|psi1(theta3)> ]
    psi12_H23 = estimator.run(circuits=[psi1, psi2, psi1], observables=[H1, H2, H3], parameter_values=[theta1, theta2, theta3])
    print(psi12_H23.result())
```
{: codeblock}

The results align with the parameter - circuit - observable tuples previously specified. For example, the first result: `EstimatorResult(values=array([1.53710938]), metadata=[{'variance': 9.249675750732422, 'shots': 1024}])` is the output of the parameter `theta1` and observable `H1` being sent to the first circuit.

Output:
```text
EstimatorResult(values=array([1.59570312]), metadata=[{'variance': 8.661632537841797, 'shots': 1024}])
EstimatorResult(values=array([-0.5390625 ,  0.07421875]), metadata=[{'variance': 0.70941162109375, 'shots': 1024}, {'variance': 1.9894332885742188, 'shots': 1024}])
EstimatorResult(values=array([0.17773438]), metadata=[{'variance': 0.9684104919433594, 'shots': 1024}])
```

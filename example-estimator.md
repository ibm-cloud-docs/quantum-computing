---

copyright:
  years: 2021
lastupdated: "2022-02-15"

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

The Estimator primitive lets you efficiently calculate and interpret expectation values of quantum operators required for many algorithms. You can specify a list of circuits and observables, then selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input.  


## Before you begin
{: #example-estimator-byb}

1. Follow the steps in [Creating and configuring a Quantum Service instance](/docs/quantum-computing?topic=quantum-computing-gettingstarted) and [Get ready to work with your Quantum Service instance](/docs/quantum-computing?topic=quantum-computing-access) to get your Quantum Service instance ready to use.

2. You'll need at least one circuit to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.

3. Create a list of observables. Observables let you define the properties of the circuit that are relevant to your problem and enable you to efficiently measure their expectation value. For simplicity, you can use the [PauliSumOp class](https://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.html#module-qiskit.opflow.primitive_ops) in Qiskit to define them.

## Inputs for Estimator
{: #Estimator-inputs}
{: step}

Estimator lets you define your jobs by using the following inputs to calculate expectation values.

These inputs can also be inspected by running the following commands:

  ```Python
   from qiskit_ibm_runtime import IBMRuntimeService

   service = IBMRuntimeService(auth="cloud", instance=<IBM Cloud CRN or Service Name>)

   program = service.program("estimator")
   print(program)
  ```
  {: note}


* The **circuits** you want to investigate.
* The **observables** you want applied to the circuits.
* The **parameter** inputs to evaluate the circuits.
* Optional: The **groupings** of circuits and observables.
* Optional: Other **run options**, such as how many **shots** to run.

### Example of preparing the required inputs:

   ```Python
   from qiskit.test.reference_circuits import ReferenceCircuits
   from qiskit_ibm_runtime import IBMRuntimeService
   from qiskit.circuit.library import EfficientSU2
   from qiskit.opflow.primitive_ops import PauliSumOp

   service = IBMRuntimeService(auth="cloud", instance=<IBM Cloud CRN or Service Name>)

   # Define the circuits

   psi1 = ReferenceCircuits.bell()
   psi2 = EfficientSU2

   # Define the observables

   H1 = PauliSumOp.from_list(
    [
      ("II", -1.052373245772859),
      ("IZ", 0.39793742484318045),
      ("ZI", -0.39793742484318045),
      ("ZZ", -0.01128010425623538),
      ("XX", 0.18093119978423156),
    ]
   )
   H2 = PauliSumOp.from_list([("IZ", 1)])
   H3 = PauliSumOp.from_list([("ZI", 1), ("ZZ", 1)])

   # Define some input parameters:

   θ1 = [0, 1, 1, 2, 3, 5]
   θ2 = [1, 2, 3, 4, 5, 6]
   θ3 = [0, 1, 1, 2, 2, 6, 3, 4]

   program_inputs =  {
      'circuits': [psi1, psi2],
      'observables': [H1, H2, H3],
      'parameters': [θ1, θ2, θ3]
      'run_options': {
        'shots': 1024
      }
   }
   options = {"backend_name": "ibm_canberra"}
   job = service.run(program_id="estimator",
      inputs=program_inputs
   )
   print(f"job id: {job.job_id}")
   result = job.result()
   print(result)

   ```

### Optional Inputs:

For a given parameter input for your circuit, you can use these optional parameters that allow you to optimize your job execution.

The **backend** lets you optionally specify the backend to run on.  If you do not specify one, the least busy backend is used.

The *grouping* input allows you to define which observable to measure for which circuit, in the format `(circuit_n, observable_m)`, where the `m<sup>th</sup>` observable you specified is measured for the `n<sup>th</sup>` circuit in the list. Using the inputs defined in the previous example, the group `(0,1)` evaluates the observable `H2` using the circuit `psi1` for some input `θ`.

This, when coupled with the shots input, lets you manage the tradeoff between speed and accuracy for calculating an expectation value for each observable over a range of parameters.

Different ways to leverage grouping:

#### Example: One parameter, one group
{: #one-parm-one-group}

 ```python
   from qiskit_ibm_runtime import IBMRuntimeService

   service = IBMRuntimeService(auth="cloud", instance=<IBM Cloud CRN>)


   # calculate [ <psi1|H1|psi1> ]
   # transpile circuits and cache for [(0, 0)]
   program_inputs =  {
      'circuits': [psi1, psi2],
      'observables': [H1, H2, H3],
      'parameters': [θ1],
      'grouping': (0,0),
   }
   options = {"backend_name": "ibm_canberra"}
   job = service.run(program_id="estimator",
      inputs=program_inputs
   )
   print(f"job id: {job.job_id}")
   H1_result = job.result()
   print("H1", H1_result)

 ```

#### Example: One parameter, multiple groups
{: #one-parm-m-group}

 ```python
   # calculate [ <psi1|H2|psi1>, <psi1|H3|psi1> ]
   # transpile circuits and cache for [(0, 1), (0, 2)]
   program_inputs =  {
      'circuits': [psi1, psi2],
      'observables': [H1, H2, H3],
      'parameters': [θ1],
      'grouping': [(0,1),(0,2)],
   }
   job = service.run(program_id="estimator",
      inputs=program_inputs
   )
   print(f"job id: {job.job_id}")
   H23_result = job.result()
   print("H2 and H3", H23_result)
 ```

#### Example: Multiple parameters, multiple groups
{: #n-parm-m-group}

```python
   # calculate [ <psi1|H1|psi1>, <psi1|H1|psi1>, <psi2|H3|psi2> ]
   program_inputs =  {
      'circuits': [psi1, psi2],
      'observables': [H1, H2, H3],
      'parameters': [θ1, θ1, θ3],
      'grouping': [(0,0),(1,2)],
   }
   job = service.run(program_id="estimator",
      inputs=program_inputs
   )
   print(f"job id: {job.job_id}")
   H13_result = job.result()
   print("H1 and H3", H13_result)
 ```

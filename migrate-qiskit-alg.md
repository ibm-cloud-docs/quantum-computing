---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-22"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, IBM Quantum Platform

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Work with updated Qiskit algorithms
{: #migrate-qiskit-alg}

The `qiskit.algorithms` module has been updated to leverage the
primitives in all of its classes.
{: shortdesc}

In practice, this means that:

1. All algorithms now take in a primitive instead of a `Backend` or `QuantumInstance`
2. Some algorithms now have a new import path
3. New primitive-specific algorithms have been introduced
4. As a side effect of the primitives refactoring, `qiskit.algorithms` no longer use `qiskit.opflow`

Using **Runtime Sessions** can be particularly advantageous when working
with variational algorithms, as they present iterative workloads that
can submit multiple jobs per iteration. On top of this, the runtime
primitives allow to try out different error mitigation techniques with
no changes to the algorithm, just a simple option configuration.

The following end-to-end example illustrates how to use one of the
refactored algorithms from `qiskit.algorithms` with the **Qiskit Runtime primitives**. For a detailed explanation of other algorithm
migration scenarios, see the [Qiskit algorithms migration guide](https://qiskit.org/documentation/stable/0.28/aqua_tutorials/Qiskit%20Algorithms%20Migration%20Guide.html){: external}.
{: note}

## Example: VQE
{: #migrate-alg-vqe}

The legacy `VQE` algorithm has been split into two new implementations:

- `VQE`: Based on Estimator
- `SamplingVQE`: For diagonal operators, based on Sampler

The choice of implementation depends on the use case - whether you are
interested in accessing the probability distribution corresponding to a
quantum state (`SamplingVQE`) or an estimation of the ground state
energy which might require, for example, measurements in multiple bases
(`VQE`).

Let's see the workflow changes for the Estimator-based VQE implementation:

### Step 1: Problem Definition
{: #migrate-alg-prob-def}

The problem definition step is common to the old and new workflow:
defining the hamiltonian, ansatz, optimizer and initial point.

The only difference is that the operator definition now relies on
`qiskit.quantum_info` instead of `qiskit.opflow` . In practice, this
means that all `PauliSumOp` dependencies should be replaced by
`SparsePauliOp`.

All of the refactored classes in `qiskit.algorithms` now take in
operators as instances of `SparsePauliOp` instead of `PauliSumOp`.
{: note}

#### The ansatz, optimizer and initial point are defined identically:
{: #migrate-alg-ansatz}

``` python
from qiskit.algorithms.optimizers import SLSQP
from qiskit.circuit.library import TwoLocal

# define ansatz and optimizer
num_qubits = 2
ansatz = TwoLocal(num_qubits, "ry", "cz")
optimizer = SLSQP(maxiter=100)

# define initial point
init_pt = [-0.1, -0.1, -0.1, -0.1, -0.1, -0.1, -0.1, -0.1]

# hamiltonian/operator --> use SparsePauliOp or Operator
from qiskit.quantum_info import SparsePauliOp

hamiltonian = SparsePauliOp.from_list(
    [
        ("II", -1.052373245772859),
        ("IZ", 0.39793742484318045),
        ("ZI", -0.39793742484318045),
        ("ZZ", -0.01128010425623538),
        ("XX", 0.18093119978423156),
    ]
)
```
{: codeblock}

#### The operator definition changes
{: #migrate-alg-operator-def}

<details>
<summary><a>Legacy VQE</a></summary>

``` python
from qiskit.opflow import PauliSumOp

hamiltonian = PauliSumOp.from_list(
    [
        ("II", -1.052373245772859),
        ("IZ", 0.39793742484318045),
        ("ZI", -0.39793742484318045),
        ("ZZ", -0.01128010425623538),
        ("XX", 0.18093119978423156),
    ]
)
```
{: codeblock}

</details>

<details>
<summary><a>New VQE</a></summary>

``` python
from qiskit.quantum_info import SparsePauliOp

hamiltonian = SparsePauliOp.from_list(
    [
        ("II", -1.052373245772859),
        ("IZ", 0.39793742484318045),
        ("ZI", -0.39793742484318045),
        ("ZZ", -0.01128010425623538),
        ("XX", 0.18093119978423156),
    ]
)
```
{: codeblock}

</details>

### Step 2: Backend setup
{: #migrate-alg-backend-setup}

Let's say that you want to run VQE on the `ibmq_qasm_simulator` in the
cloud. Before you would load you IBMQ account, get the corresponding
backend from the provider, and use it to set up a `QuantumInstance`.
Now, you need to initialize a `QiskitRuntimeService`, open a
[session](https://quantum-computing.ibm.com/lab/docs/iql/manage/systems/sessions)
and use it to instantiate your `.Estimator`.

<details>
<summary><a>Legacy VQE</a></summary>

``` python
from qiskit.utils import QuantumInstance
from qiskit import IBMQ

IBMQ.load_account()
provider = IBMQ.get_provider(hub='MY_HUB')
my_backend = provider.get_backend("ibmq_qasm_simulator")
qi = QuantumInstance(backend=my_backend)
```
{: codeblock}

</details>

<details>
<summary><a>New VQE</a></summary>

``` python
from qiskit_ibm_runtime import Estimator, QiskitRuntimeService, Session

# no more IBMQ import or .load_account()
service = QiskitRuntimeService(channel="ibm_quantum")
session = Session(service, backend="ibmq_qasm_simulator") # open session
estimator = Estimator(session = session)
```
{: codeblock}

</details>

### Step 3: Run VQE
{: #migrate-alg-vqe-run}

Now that both the problem and the execution path have been set up, you
can instantiate and run VQE. Close the session only if all jobs are
finished and you don\'t need to run more jobs in the session.

`VQE` is one of the algorithms with a changed import path. If you do not
specify the full path during the import, you might run into conflicts
with the legacy code.
{: #important}

<details>
<summary><a>Legacy VQE</a></summary>

``` python
from qiskit.algorithms.minimum_eigen_solvers import VQE

vqe = VQE(ansatz, optimizer, quantum_instance=qi)
result = vqe.compute_minimum_eigenvalue(hamiltonian)
```
{: codeblock}

</details>

<details>
<summary><a>New VQE</a></summary>

``` python
# note change of namespace
from qiskit.algorithms.minimum_eigensolvers import VQE

vqe = VQE(estimator, ansatz, optimizer)
result = vqe.compute_minimum_eigenvalue(hamiltonian)

# close session after all jobs have completed
session.close()
```
{: codeblock}

</details>

### Using context managers
{: #migrate-alg-vqe-context-man}

We recommend that you initialize your primitive and run your algorithm
using a context manager. The code for steps 2 and 3 would then look
like this:

``` python
from qiskit_ibm_runtime import Estimator, QiskitRuntimeService, Session
from qiskit.algorithms.minimum_eigensolvers import VQE

service = QiskitRuntimeService(channel="ibm_quantum")

with Session(service, backend="ibmq_qasm_simulator") as session:

    estimator = Estimator() # no need to pass the session explicitly
    vqe = VQE(estimator, ansatz, optimizer, gradient=gradient, initial_point=init_pt)
    result = vqe.compute_minimum_eigenvalue(hamiltonian)
```
{: codeblock}

## Related links
{: #migrate-alg-rel}

- See the [Session documentation](/docs/quantum-computing?topic=quantum-computing-sessions) for further information about the Qiskit Runtime sessions.
- See the [How to run a primitive in a session](https://docs.quantum-computing.ibm.com/run/run-sessions){: external} topic for detailed code examples.
- See the [Qiskit algorithm documentation](https://qiskit.org/documentation/apidoc/algorithms.html){: external} for details about each algorithm.
- See the [Qiskit algorithm tutorials](https://qiskit.org/documentation/tutorials.html#algorithms){: external} for examples of how to use algorithms.
- Read the blog [Introducing Qiskit Algorithms With Qiskit Primitives!](https://medium.com/qiskit/introducing-qiskit-algorithms-with-qiskit-runtime-primitives-d89703ecfca3){: external} for an introduction to using the updated algorithms.

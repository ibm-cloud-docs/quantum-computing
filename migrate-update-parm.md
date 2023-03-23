---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-22"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, IBM Quantum Platform

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Parametrized Circuits With Primitives
{: #migrate-circuits-update}

Parametrized circuits are a commonly used tool for quantum algorithm
design. Because `backend.run()` did not accept parametrized
circuits, the parameter binding step had to be integrated in the
algorithm workflow. The primitives can perform the parameter binding
step internally, which results in a simplification of the algorithm-side
logic.
{: shortdesc}

The following example summarizes the new workflow for managing
parametrized circuits.

## Example
{: #update-parm-example}

Let's define a parametrized circuit:

``` python
from qiskit.circuit import QuantumCircuit, ParameterVector

n = 3
thetas = ParameterVector('θ',n)

qc = QuantumCircuit(n, 1)
qc.h(0)

for i in range(n-1):
    qc.cx(i, i+1)

for i,t in enumerate(thetas):
    qc.rz(t, i)

for i in reversed(range(n-1)):
    qc.cx(i, i+1)

qc.h(0)
qc.measure(0, 0)

qc.draw()
```
 {: codeblock}

We want to assign the following parameter values to the circuit:

``` python
import numpy as np
theta_values = [np.pi/2, np.pi/2, np.pi/2]
```
{: codeblock}

## Legacy
{: #update-parm-legacy}

Previously, the parameter values had to be bound to their respective
circuit parameters prior to calling `backend.run`.

``` python
from qiskit import Aer

bound_circuit = qc.bind_parameters(theta_values)
bound_circuit.draw()

backend = Aer.get_backend('aer_simulator')
job = backend.run(bound_circuit)
counts = job.result().get_counts()
print(counts)
```
{: codeblock}

## Updated (primitives)
{: #update-parm-updated}

Now, the primitives take in parametrized circuits directly, together
with the parameter values, and the parameter assignment operation can be
performed more efficiently on the server side of the primitive.

This feature is particularly interesting when working with iterative
algorithms because the parametrized circuit remains unchanged between
calls while the parameter values change. The primitives can transpile
once and then cache the unbound circuit, using classical resources more
efficiently. Moreover, only the updated parameters are transferred to
the cloud, saving additional bandwidth.

``` python
from qiskit.primitives import Sampler

sampler = Sampler()
job = sampler.run(qc, theta_values)
result = job.result().quasi_dists
print(result)
```
{: codeblock}


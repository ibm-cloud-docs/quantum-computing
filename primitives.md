---

copyright:
  years: 2021, 2022
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing


---

{{site.data.keyword.attribute-definition-list\}\}


# Primitive programs
{: #primitive-programs}

With qiskit runtime, we are introducing a new set of interfaces, in the form of primitive programs, to run jobs on quantum computers. The existing Qiskit interface to backends (`backend.run()`) was originally designed to accept a list of circuits and return shot counts for every job.

Over time, it became clear that users have diverse purposes for quantum computing, and therefore the ways in which they define the requirements for their computing jobs are expanding. Consequently, their results also look different. For example, a user doing algorithm research and development cares about information beyond counts; they are more focused on efficiently calculating quasi-probabilities and expectation values of observables.
{: shortdesc}

These primitives are designed to provide methods that make building modular algorithms and other higher order programs easier. They will provide a seamless way to leverage the latest optimizations in IBM Quantum hardware and software.   

With our first set of primitive programs, we enable capabilities that allow users to extract more performance out of the Qiskit runtime service.  Introducing Sampler and Estimator:

## Available primitives
{: #available-primitives}

* Estimator: Allows users to efficiently calculate and interpret expectation values of quantum operators required for many algorithms. Users specify a list of circuits and observables, then tell the program how to selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input.
* Sampler: Allows users to more accurately contextualize counts. It takes a user circuit as an input and generates an error mitigated readout of quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference. 

## How to use primitives
{: #how-to-use-primitives}

Primitives work like any other program.  You generate one or more circuits by using Qiskit, then send those circuits, along with any other configuration values to the primitive. The primitive takes care of sending the circuits to the quantum system, error mitigation, processing the circuits, then returning results.  For full details and examples, refer to the topics about each primitive.



## More information
{: #more-information}

- [Get started with Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator)
- [Get started with Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler)
- [Estimator reference](/docs/quantum-computing?topic=quantum-computing-program-estimator)
- [Sampler reference](/docs/quantum-computing?topic=quantum-computing-program-sampler)
- Use [tutorials](https://qiskit.org/documentation/tutorials.html) to learn how to create circuits with Qiskit.

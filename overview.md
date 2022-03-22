---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-01"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}


# Overview of primitive programs
{: #overview}


With {{site.data.keyword.qiskit_runtime_notm}}, we are introducing a new set of interfaces, in the form of primitive programs, to run jobs on quantum computers. The existing Qiskit interface to backends (`backend.run()`) was originally designed to accept a list of circuits and return shot counts for every job.

Over time, it became clear that users have diverse purposes for quantum computing, and therefore the ways in which they define the requirements for their computing jobs are expanding. Consequently, their results also look different. For example, a user doing algorithm research and development cares about information beyond counts; they are more focused on efficiently calculating quasiprobabilities and expectation values of observables.

These primitives are designed to provide methods that make it easier to build modular algorithms and other higher-order programs. They will provide a seamless way to leverage the latest optimizations in IBM Quantum hardware and software.   

With our first set of primitive programs, we enable capabilities that allow users to extract more performance out of the {{site.data.keyword.qiskit_runtime_notm}} service.  Introducing Sampler and Estimator:

## Available primitives
{: #available-primitives}

* **Sampler**: Allows users to more accurately contextualize counts. It takes a user circuit as an input and generates an error-mitigated readout of quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference. 
* **Estimator**: Allows users to efficiently calculate and interpret expectation values of quantum operators required for many algorithms. Users specify a list of circuits and observables, then tell the program how to selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input.

## How to use primitives
{: #how-to-use-primitives}

Primitive program interfaces vary based on result type. Once you have identified the appropriate primitive for your program, you can use Qiskit to prepare inputs, such as circuits, observables (for Estimator), and customizable options that allow you optimize your job.  For full details and examples, refer to the topics about each primitive:

For examples of using primitives, see [Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler) or [Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator).


## Next steps
{: #next-steps}

- Use the [Quick start guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to create an instance and run your first job.
- Use Qiskit [tutorials](https://qiskit.org/documentation/tutorials.html){: external} to learn how to create circuits with Qiskit.
- View the [Qiskit Runtime API reference](/apidocs/quantum-computing){: external} to understand how to use cURL commands to work with your cloud service instance.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}

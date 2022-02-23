---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing


---

{{site.data.keyword.attribute-definition-list}}

# Quantum Services overview
{: #overview}

Get a glimpse of the quantum computing future with our world-leading {{site.data.keyword.quantum_long_notm}}, which leverages Qiskit Runtime, a new architecture that delivers significant performance enhancements to program execution. Our physical systems and simulators (cloud-based classical emulators of quantum systems) now enable you to experience frictionless quantum computing; that is, the ability to execute quantum programs in an environment where the classical computer is physically closer to the quantum computer.  Test programs and algorithms, and develop new models with our cloud-based quantum runtime for drastically improved capacity and higher performance today.
{: shortdesc}

Because this service is in Experimental phase, many functions are not yet available and are still under development. This includes some of the functions outlined in the following diagram.
{: note}


![A user is shown running the Qiskit program via APIs that control the Qiskit Runtime Manager.](images/Qiskit_Runtime_architecture.png "Qiskit Runtime architecture diagram"){: caption="Figure 1. Diagram of Qiskit Runtime's architecture" caption-side="bottom"}


## Why use Qiskit Runtime?
{: #why}

**Run your experiments with an improved architecture**

For variational algorithms such as VQE, the loops between classical and quantum computation will happen with a low-latency connection to the quantum device.

**Use primitives to get started quickly**

Primitive programs provide a simplified interface for building and customizing applications. You can submit circuits and return shot counts, pre-shot readouts, or observable expectation values. (Some primitives are future functions.)

**Upload and iterate**

Upload your own Qiskit quantum program and run it with different inputs and configuration each time. (Future functionality)

**Receive intermediate results**

Receive intermediate results as your execution runs. (Future functionality)

## Methods for interacting with Qiskit Runtime
{: #methods}

You can work with this service by using our Qiskit Runtime library Python commands (preferred) or by using curl to access the Quantum Cloud Runtime API directly. You should use the method that is most familiar to you, but if you don't have a specific reason for using curl, Python  is typically easier.


## Overview of primitive programs
{: #primitive-programs}

With Qiskit runtime, we are introducing a new set of interfaces, in the form of primitive programs, to run jobs on quantum computers. The existing Qiskit interface to backends (`backend.run()`) was originally designed to accept a list of circuits and return shot counts for every job.

Over time, it became clear that users have diverse purposes for quantum computing, and therefore the ways in which they define the requirements for their computing jobs are expanding. Consequently, their results also look different. For example, a user doing algorithm research and development cares about information beyond counts; they are more focused on efficiently calculating quasi-probabilities and expectation values of observables.

These primitives are designed to provide methods that make building modular algorithms and other higher order programs easier. They will provide a seamless way to leverage the latest optimizations in IBM Quantum hardware and software.   

With our first set of primitive programs, we enable capabilities that allow users to extract more performance out of the Qiskit runtime service.  Introducing Sampler and Estimator:

## Available primitives
{: #available-primitives}

* Estimator: Allows users to efficiently calculate and interpret expectation values of quantum operators required for many algorithms. Users specify a list of circuits and observables, then tell the program how to selectively group between the lists to efficiently evaluate expectation values and variances for a given parameter input.
* Sampler: Allows users to more accurately contextualize counts. It takes a user circuit as an input and generates an error mitigated readout of quasiprobabilities. This enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference. 

## How to use primitives
{: #how-to-use-primitives}

Primitives work like any other program.  You generate one or more circuits by using Qiskit, then send those circuits, along with any other configuration values to the primitive. The primitive takes care of sending the circuits to the quantum system, error mitigation, processing the circuits, then returning results.  For full details and examples, refer to the topics about each primitive.

For examples of using primitives, see [Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator) or  [Sampler](/docs/quantum-computing?topic=quantum-computing-example-sampler).


## Next steps
{: #next-steps}

- Use the [Quick start guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to create an instance and run your first job.
- Use Qiskit [tutorials](https://qiskit.org/documentation/tutorials.html){: external} to learn how to create circuits with Qiskit.
- View the [Quantum Computing API reference](/apidocs/quantum-computing){: external} to understand how to use cURL commands to work with your cloud service instance.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}
---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-29"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, Qiskit 1.0

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Qiskit Runtime overview
{: #overview}

Get a glimpse of the quantum computing future with our world-leading {{site.data.keyword.qiskit_runtime_notm}}, a new architecture that delivers significant performance enhancements to program execution. By using our physical QPUs and simulators (classical emulators of QPUs), you can experience frictionless quantum computing. That is, the ability to run quantum programs in an environment where the classical computer is physically closer to the quantum computer. Test programs and algorithms, and develop new models with our cloud-based quantum runtime for drastically improved capacity and higher performance.
{: shortdesc}

This documentation is based on the current version of [Qiskit Runtime.](https://docs.quantum.ibm.com/api/qiskit-ibm-runtime){: external}

Because this service is in Beta phase, many functions are not yet available and are still under development, including some functions that are outlined in the following diagram.
{: note}

![The top box represents code, which consists of classical + quantum processes. The user sends the code to the Qiskit Runtime API, which is part of the Qiskit Runtime Service. Primitive programs are part of the API. The API sends QASM circuits to quantum hardware, which returns the circuits back to the API.  All of this processing is part of the Qiskit Runtime Service. The API then sends return values back to the user.](images/Qiskit_Runtime_architecture.svg "Qiskit Runtime architecture diagram"){: caption="Diagram of Qiskit Runtime's architecture" caption-side="bottom"}

## Why use Qiskit Runtime?
{: #why}

Run your experiments with an improved architecture:   For variational algorithms such as VQE, the loops between classical and quantum computation happen with a low-latency connection to the quantum device.

Use primitives to get started quickly:   Primitive programs provide a simplified interface for building and customizing applications. You can submit circuits and return shot counts, pre-shot readouts, or observable expectation values. (Some primitives are future functions.)

## Overview of primitive programs
{: #primitive-programs}

Qiskit Runtime introduces a set of interfaces, in the form of primitive programs, to expand on how users run jobs on quantum computers.

The existing Qiskit interface to backends (`backend.run()`) was designed to accept a list of circuits and return counts for every job. Over time, it became clear that users have diverse purposes for quantum computing, and therefore the ways in which they define the requirements for their computing jobs are expanding. Therefore, their results also look different.

For example, an algorithm researcher and developer cares about information beyond counts; they are more focused on efficiently calculating quasiprobabilities and expectation values of observables.

Our primitives provide methods that make it easier to build modular algorithms and other higher-order programs. Instead of simply returning counts, they return more immediately meaningful information. Additionally, they provide a seamless way to access the latest optimizations in IBM Quantum hardware and software.

The basic operations that one can do with a probability distribution is to sample from it or to estimate quantities on it. Therefore, these operations form the fundamental building blocks of quantum algorithm development. The Qiskit Runtime primitives (Sampler and Estimator) use these sampling and estimating operations as core interfaces to our QPUs. Learn more about what you can do with Qiskit Runtime primitive programs in the [IBM Quantum documentation.](https://docs.quantum.ibm.com/guides){: external}

To ensure faster and more efficient results, as of 1 March 2024, circuits and observables need to be transformed to only use instructions supported by the QPU (referred to as *instruction set architecture (ISA)* circuits and observables) before being submitted to the Qiskit Runtime primitives. See the [transpilation documentation](https://docs.quantum.ibm.com/guides){: external} for instructions to transform circuits.
{: important}

This change has the following important impacts:

*  Because transpilation is done to match the circuits available on a specific backend, you **must** specify a backend.  The option to use the least busy QPU that you have access to will not work.  If you don't specify a backend, you will receive an error.
*  The primitives will no longer perform layout or routing operations. Consequently, transpilation options referring to those tasks will no longer have any effect. By default, all primitives except Sampler V2 still optimize the input circuits. To bypass all optimization, set `optimization_level=0`.

## Available primitives
{: #available-primitives}

The following primitive programs are available:

| Primitive | Description | Example output |
|---|---|---|
| Sampler | Allows a user to input a circuit and then return the outputs (bitstrings) from every shot (V2), or quasiprobabilities (V1). This generation enables users to more efficiently evaluate the possibility of multiple relevant data points in the context of destructive interference. | ![An example of Sampler output is shown.](images/sampler.png) |
| Estimator | Allows a user to specify a list of circuits and observables and selectively group between the lists to efficiently evaluate expectation values and variances for a parameter input. It is designed to enable users to efficiently calculate and interpret expectation values of quantum operators that are required for many algorithms. | ![An example of Estimator output is shown.](images/estimator.png) |
{: caption="Available primitive programs" caption-side="bottom"}

## How to use primitives
{: #how-to-use-primitives}

Primitive program interfaces vary based on the type of task that you want to run on the quantum computer and the corresponding data that you want returned as a result. After identifying the appropriate primitive for your program, you can use Qiskit to prepare inputs, such as circuits, observables (for Estimator), and customizable options to optimize your job.


## V2 primitives
{: #v2-primitives}

This document uses Version 2 primitives (available with qiskit-ibm-runtime 0.21.0).  Version 2 is first the major interface change since the introduction of Qiskit Runtime primitives. Based on user feedback, this version introduces the following major new functions:

Version 2 of the primitives is introduced with a new base class for both Sampler and Estimator ([BaseSamplerV2](https://docs.quantum.ibm.com/api/qiskit/qiskit.primitives.BaseSamplerV2){: external} and [BaseEstimatorV2](https://docs.quantum.ibm.com/api/qiskit/qiskit.primitives.BaseEstimatorV2){: external}), along with new types for their inputs and outputs.

The new interface lets you specify a single circuit and multiple observables (if using Estimator) and parameter value sets for that circuit, so that sweeps over parameter value sets and observables can be efficiently specified. Previously, you had to specify the same circuit multiple times to match the size of the data to be combined.  Also, while you can still use `optimization_level` and `resilience_level` (if using Estimator) as the simple knobs, V2 primitives give you the flexibility to turn on or off individual error mitigation / suppression methods to customize them for your needs.

To reduce the total job execution time, V2 primitives only accept circuits and observables that use instructions supported by the target QPU (referred to as instruction set architecture (ISA) circuits and observables). V2 primitives do not perform layout, routing, and translation operations but continue to optimize the circuits if you specify `optimization_level>0`.  See the [transpilation documentation](https://docs.quantum.ibm.com/guides){: external} for instructions to transform circuits.

V2 Sampler is simplified to focus on its core task of sampling the output register from execution of quantum circuits. It returns the samples, whose type is defined by the program, without weights. The output data is also separated by the output register names defined by the program. This change enables future support for circuits with classical control flow.

To learn more, refer to the [V2 primitives migration guide.](https://docs.quantum.ibm.com/api/migration-guides/v2-primitives){: external}


## Next steps
{: #next-steps-overview}

- Use the [Getting started guide](/docs/quantum-computing?topic=quantum-computing-get-started) to create an instance and run your first job.
- [Get started with primitives](https://docs.quantum.ibm.com/guides/primitives-get-started){: external}
- [Learn by using tutorials, courses, and tools](https://learning.quantum.ibm.com){: external}
- Watch the [Qiskit Runtime Tutorial video](https://www.youtube.com/watch?v=b9mdMye-iVk){: external} for more information.
- View the [Qiskit Runtime API reference](/apidocs/quantum-computing){: external} to understand how to use cURL commands to work with your cloud service instance.
- Learn about [IBM Quantum Computing](https://www.ibm.com/quantum/){: external}.

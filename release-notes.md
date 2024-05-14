---

copyright:
  years: 2021, 2023
lastupdated: "2024-05-14"
content-type: release-note
subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Release notes for Qiskit Runtime
{: #quantum-computing-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.qiskit_runtime_notm}}.
{: shortdesc}

## 8 May 2024
{: #0.8.0}
{: release-note}

The Qiskit Runtime CLI and its documentation have been removed.

## 29 April 2024
{: #0.8.0}
{: release-note}

Sessions run in _dedicated_ mode, which might increase the cost of running a session.  For information, refer to the [Execution modes](https://docs.quantum.ibm.com/run/execution-modes){: external} documentation.

## 1 March 2024
{: #0.7.0}
{: release-note}

The latest update for Qiskit Runtime includes V2 primitives, which require updated code formatting. For information, refer to [Migrate to the Qiskit Runtime V2 primitives.](https://docs.quantum.ibm.com/api/migration-guides/v2-primitives){: external}

## 15 February 2024
{: #0.6.0}
{: release-note}

The primitives now require ISA input.  Because this formating is specific to each backend, you must specify a backend.

## 14 August 2023
{: #0.5.0}
{: release-note}

The max_execution_time value is based on job execution time instead of wall clock time. Job execution time represents the time that the QPU complex (including control software, control electronics, QPU, and so on) is engaged in processing the job.

## 13 January 2023
{: #0.4.0}
{: release-note}

The Standard plan now includes access to simulators.

## 19 October 2022
{: #0.3.0}
{: release-note}

The Estimator and Sampler primitives interface is redesigned.

## 12 April 2022
{: #0.2.0}
{: release-note}

Qiskit Runtime achieves Beta maturity in IBM Cloud, and introduces pay-as-you-go for the standard plan.

## 15 February 2022
{: #0.1.0}
{: release-note}

Initial release of primitive programs.

## 31 January 2022
{: #0.0.0}
{: release-note}

You can run jobs on both physical quantum devices, as well as quantum device simulators.
New API endpoints have been added to allow you to view device information.
There is a free plan as well as a lite plan, to allow access to simulators or physical quantum devices.
If you do not specify a device for your job to run on, the job will be sent to the least busy system or simulator that you have access to.

## 27 November 2021
{: #1.0.0}
{: release-note}

Introducing {{site.data.keyword.qiskit_runtime_notm}} (internal-only). Get a glimpse of the quantum computing future with our world-leading Qiskit Runtime, a new architecture that delivers significant performance enhancements to program execution. Our physical systems and simulators (cloud-based classical emulators of quantum systems) now enable you to experience frictionless quantum computing; that is, the ability to execute quantum programs in an environment where the classical computer is physically closer to the quantum computer. Test programs and algorithms, and develop new models with our cloud-based quantum runtime for drastically improved capacity and higher performance today.

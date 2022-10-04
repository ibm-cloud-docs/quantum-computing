---

copyright:
  years: 2022
lastupdated: "2022-10-04"



keywords: quantum, Qiskit, runtime, near time compute, standard plan, pay-as-you-go, lite plan

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Qiskit Runtime plans
{: #cost}

The Qiskit Runtime service offers these plans for running quantum programs:
- Lite Plan: Simulator access plan (free)
- Standard Plan: Quantum hardware access plan
{: shortdesc}

## Lite plans
{: #lite-plan-details}

A free plan that gives you access to quantum simulators to help you get started with Qiskit Runtime. It does not include access to IBM Quantum systems. The following simulators are included in this plan:


- **ibmq_qasm_simulator**: A general-purpose simulator for simulating quantum circuits both ideally and subject to noise modeling. The simulation method is automatically selected based on the input circuits and parameters.
   - **Type**: General, context-aware
   - **Simulatable Qubits**: 32
- **simulator_statevector**: Simulates a quantum circuit by computing the wave function of the qubit’s state vector as gates and instructions are applied. Supports general noise modeling.
    - **Type**: Schrödinger wave function
    - **Simulated Qubits**: 32
- **simulator_mps**: A tensor-network simulator that uses a Matrix Product State (MPS) representation for the state that is often more efficient for states with weak entanglement.
   - **Type**: Matrix Product State
   - **Simulated Qubits**: 100
- **simulator_stabilizer**: An efficient simulator of Clifford circuits. Can simulate noisy evolution if the noise operators are also Clifford gates.
   - **Type**: Clifford
   - **Simulated Qubits**: 5000
- **simulator_extended_stabilizer**: Approximates the action of a quantum circuit by using a ranked-stabilizer decomposition. The number of non-Clifford gates determines the number of stabilizer terms.
   - **Type**: Extended Clifford (for example, Clifford+T)
   - **Simulated Qubits**: 63

## Standard plan
{: #standard-plan}

A pay-as-you-go plan for accessing IBM Quantum systems. Build your own programs and access all the benefits of Qiskit Runtime by running on real quantum hardware.

## Pricing overview
{: #pricing-overview}

The Lite plan is free. The Standard plan charges you per _runtime second_. The following diagram illustrates what is included in a runtime second. For this service, one runtime second includes quantum compute time as well as classical near-time pre- and post-processing time. Any time spent waiting for results or in the queue for the quantum computer are excluded from the classical processing time.

![This diagram shows that everything before the program starts (such as queuing) is free. After the job starts, it costs $1.60 per second.](images/Runtime_Accounting_Diagram.png "Runtime second accounting"){: caption="Figure 1. Runtime second accounting" caption-side="bottom"}

The quantum and near-time classical compute times are grouped to deliver the best performance of quantum programs. The runtime environment uses near-time classical compute to host in-term hardware-dependent tasks like session management, circuit optimization, and (when supported) error mitigation to optimize the execution of quantum programs.

## Time limits on programs
{: #time-limits}

The maximum execution time for the Sampler primitive is 10000 seconds (2.78 hours). The maximum execution time for the Estimator primitive is 18000 seconds (5 hours).

Additionally, the system limit on the job execution time is 3 hours for a job that is running on a simulator and 8 hours for a job running on a physical system.



## How to limit your cost
{: #limit-cost}

The time your job takes (and therefore, its cost) depends on how many iterations you make in a session and how many shots are run in each iteration. Thus, you can manage your cost by running only as many iterations and shots as you need. As we expand on the primitives interface, we will provide more ways to manage cost.

## How to see what you're being charged
{: #pricing-bill}

You are sent a monthly invoice that provides details about your resource charges. You can check how much has been spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing). You can also see how much has been spent on each instance's details page. For more information, see [View the cost](/docs/quantum-computing?topic=quantum-computing-view-cost).


## Set up spending notifications
{: #pricing-notifications}

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account **Type** description](https://cloud.ibm.com/docs/account?topic=account-accounts){: external}. {{site.data.keyword.cloud}} spending notifications must be used with other methods of cost management for several reasons:

   - The notifications trigger only _after_ cost surpasses the specified limit.
   - Cost is submitted to the billing system hourly. Thus, a long delay might occur between the job submission and the spending notification being sent.
   - The billing system can take multiple days to get information to the invoicing system, which might cause further delay in notifications. For more information about how the IBM Cloud billing system works, see [Setting spending notifications](https://cloud.ibm.com/docs/billing-usage?topic=billing-usage-spending){: external}.

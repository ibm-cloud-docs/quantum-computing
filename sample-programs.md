---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute, quantum program

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}

# Available sample programs
{: #sample-programs}

This product is currently only available internally.  Get notified about {{site.data.keyword.quantum_long_notm}} public experimental release by filling out [this form](https://airtable.com/shrRpebS4aD3XeDhA){: external}.
{: note}

The sample Quantum runtime programs are publicly available.  This topic lists the necessary information to interact with each.
- [circuit-runner](#runner)
- [quantum-kernel-alignment](#quantum-kernel-alignment)
- [sample-program](#sample)
- [sampler](#sampler)
- [vqe](#vqe)
{: shortdesc}


## circuit-runner
{: #runner}

A runtime program that takes one or more circuits, compiles them, executes them, and optionally applies measurement error mitigation.

- **Maximum execution time**: 1800 seconds (30 minutes)
- **Input parameters**:  
    - **circuits**:
        - **Description**: A circuit or a list of circuits.
        - **Type**: A QuantumCircuit or a list of QuantumCircuits.
        - **Required**: True
    - **shots**:
        - **Description**: Number of repetitions of each circuit, for sampling. Default: 1024.
        - **Type**: int
        - **Required**: False
    - **initial_layout**:
        - **Description**: Initial position of virtual qubits on physical qubits.
        - **Type**: dict or list
        - **Required**: False
    - **layout_method**:
        - **Description**: Name of layout selection pass ('trivial', 'dense', 'noise_adaptive', 'sabre')
        - **Type**: string
        - **Required**: False
    - **routing_method**:
        - **Description**: Name of routing pass ('basic', 'lookahead', 'stochastic', 'sabre').
        - **Type**: string
        - **Required**: False
    - **translation_method**:
        - **Description**: Name of translation pass ('unroller', 'translator', 'synthesis').
        - **Type**: string
        - **Required**: False
    - **seed_transpiler**:
        - **Description**: Sets random seed for the stochastic parts of the transpiler.
        - **Type**: int
        - **Required**: False
    - **optimization_level**:
        - **Description**: How much optimization to perform on the circuits (0-3). Higher levels generate more optimized circuits. Default is 1.
        - **Type**: int
        - **Required**: False
    - **init_qubits**:
        - **Description**: Whether to reset the qubits to the ground state for each shot.
        - **Type**: bool
        - **Required**: False
    - **rep_delay**:
        - **Description**: Delay between programs in seconds.
        - **Type**: float
        - **Required**: False
    - **transpiler_options**:
        - **Description**: More compilation options.
        - **Type**: dict
        - **Required**: False
    - **measurement_error_mitigation**:
        - **Description**: Whether to apply measurement error mitigation. Default is False.
        - **Type**: bool
        - **Required**: False  
- **Interim results**:  
    **none**  
- **Returns**:  
        - **Description**: Circuit execution results.  
        - **Type**: RunnerResult object  


## quantum-kernel-alignment
{: #quantum-kernel-alignment}

Quantum kernel alignment algorithm that learns, on a specific data set, a quantum kernel that maximizes the SVM classification margin.

- **Maximum execution time**: 28800 seconds (8 hours)
- **Input parameters**:
    - **feature_map**:
        - **Description**: An instance of FeatureMap in dictionary format used to map classical data into a quantum state space.
        - **Type**: dict
        - **Required**: True
    - **data**:
        - **Description**: NxD array of training data, where N is the number of samples and D is the feature dimension.
        - **Type**: numpy.ndarray
        - **Required**: True
    - **labels**:
        - **Description**: Nx1 array of +/-1 labels of the N training samples.
        - **Type**: numpy.ndarray
        - **Required**: True
    - **initial_kernel_parameters**:
        - **Description**: Initial parameters of the quantum kernel. If not specified, an array of randomly generated numbers is used.
        - **Type**: numpy.ndarray
        - **Required**: False
    - **maxiters**:
        - **Description**: Number of SPSA optimization steps. Default is 1.
        - **Type**: int
        - **Required**: False
    - **C**:
        - **Description**: Penalty parameter for the soft-margin support vector machine. Default is 1.
        - **Type**: float
        - **Required**: False
    - **initial_layout**:
        - **Description**: Initial position of virtual qubits on the physical qubits of the quantum device. Default is None.
        - **Type**: list or dict
        - **Required**: False
- **Interim results**:
    none
- **Returns**:
    - **aligned_kernel_parameters**:  
        - **Description**: The optimized kernel parameters found from quantum kernel alignment.
        - **Type**: numpy.ndarray
    - **aligned_kernel_matrix**:  
        - **Description**: The aligned quantum kernel matrix evaluated with the optimized kernel parameters on the training data.
        - **Type**: numpy.ndarray


## sample-program
{: #sample}

A sample runtime program.
- **Maximum execution time**: 300 seconds (5 minutes)
- **Input parameters**:
    - **iterations**:
        - **Description**: Number of iterations to run. Each iteration generates and runs a random circuit.
        - **Type**: int
        - **Required**: True
- **Interim results**:
    - **iteration**:  
        - **Description**: Iteration number.  
        - **Type**: int
    - **counts**:  
        - **Description**: Histogram data of the circuit result.  
        - **Type**: dict
- **Returns**:  
        - **Description**: A string that says 'All done!'.  
        - **Type**: string


## sampler
{: sampler}

Sample distributions generated by given circuits executed on the target backend.
- **Maximum execution time**: 10000 seconds (2.78 hours)
- **Input parameters**:
    - **circuits**:
        - **Description**: A QuantumCircuit or list of QuantumCircuits.
        - **Type**: QuantumCircuit or list
        - **Required**: True
    - **transpiler_config**:
        - **Description**: A collection of kwargs passed to transpile.
        - **Type**: dict
        - **Required**: False
    - **run_config**:
        - **Description**: A collection of kwargs passed to backend.run.
        - **Type**: dict
        - **Required**: False
    - **skip_transpilation**:
        - **Description**: Skip circuit transpilation.
        - **Type**: bool
        - **Required**: False
    - **use_measurement_mitigation**:
        - **Description**: Use measurement mitigation to improve results.
        - **Type**: bool
        - **Required**: False
    - **return_mitigation_overhead**:
        - **Description**: Return mitigation overhead factor.
        - **Type**: bool
        - **Required**: False
- **Interim results**:  
    none
- **Returns**:
    - **raw_counts**:  
        - **Description**: A list of counts data for each circuit passed.
        - **Type**: list
    - **quasiprobabilities**:  
        - **Description**: Quasiprobabilities for each circuit if using measurement mitigation.
        - **Type**: list
    - **mitigation_overhead**:  
        - **Description**: Mitigation overheads.
        - **Type**: list



## vqe
{: #vqe}

Variational Quantum Eigensolver (VQE) to find the minimal eigenvalue of a Hamiltonian.
- **Maximum execution time**: 14400 seconds (4 hours)
- **Input parameters**:
    - **ansatz**:
        - **Description**: A parameterized quantum circuit that parameterizes the ansatz wavefunction for the VQE. It is assumed that all qubits are initially in the 0 state.
        - **Type**: QuantumCircuit
        - **Required**: True
    - **operator**:
        - **Description**: The Hamiltonian whose smallest eigenvalue we're trying to find.
        - **Type**: PauliSumOp
        - **Required**: True
    - **optimizer**:
        - **Description**: The classical optimizer used in to update the parameters in each iteration. Currently, only SPSA and QN-SPSA are supported. The value must be a dictionary that specifies the name and options of the optimizer, for example: ``{'name': 'SPSA', 'maxiter': 100}``.
        - **Type**: dict
        - **Required**: True
    - **initial_parameters**:
        - **Description**: Initial parameters of the ansatz. Can be an array or the string ``'random'`` to choose random initial parameters.
        - **Type**: Union[numpy.ndarray, str]
        - **Required**: True
    - **aux_operators**:
        - **Description**: A list of operators to be evaluated at the final, optimized state.
        - **Type**: List[PauliSumOp]
        - **Required**: False
    - **shots**:
        - **Description**: The number of shots used for each circuit evaluation. Defaults to 1024.
        - **Type**: int
        - **Required**: False
    - **readout_error_mitigation**:
        - **Description**: Whether to apply readout error mitigation in form of a complete measurement fitter to the measurements. Defaults to False.
        - **Type**: bool
        - **Required**: False
    - **initial_layout**:
        - **Description**: Initial position of virtual qubits on the physical qubits of the quantum device. Default is None.
        - **Type**: list or dict
        - **Required**: False
- **Interim results**:  
    none
- **Returns**:
    - **optimizer_evals**:  
        - **Description**: The number of steps of the optimizer.
        - **Type**: int
    - **optimizer_time**:
        - **Description**: The total time taken by the optimizer.
        - **Type**: float
    - **optimal_value**:
        - **Description**: The smallest value found during the optimization. Equal to the ``eigenvalue`` attribute.
        - **Type**: float
    - **optimal_point**:
        - **Description**: The optimal parameter values found during the optimization.
        - **Type**: np.ndarray
    - **optimal_parameters**:
        - **Description**: Not supported currently.
        - **Type**: NoneType
    - **cost_function_evals**:
        - **Description**: The number of cost function (energy) evaluations
        - **Type**: int
    - **eigenstate**:
        - **Description**: The square root of sampling probabilities for each computational basis state of the circuit with optimal parameters.
        - **Type**: dict
    - **eigenvalue**:
        - **Description**: The estimated eigenvalue.
        - **Type**: complex
    - **aux_operator_eigenvalues**:
        - **Description**: The expectation values of the auxiliary operators at the optimal state.
        - **Type**: np.ndarray
    - **optimizer_history**:
        - **Description**: A dictionary that contains information about the optimization process: the value objective function, parameters, and a timestamp.
        - **Type**: dict


## Related information
{: #next-steps}

- [Submit a job](/docs/quantum-computing?topic=quantum-computing-run_job)
- View the [API reference](/apidocs/quantum-computing){: external}
- Learn about IBM Quantum:
    - [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}
    - [Qiskit](https://qiskit.org/){: external}

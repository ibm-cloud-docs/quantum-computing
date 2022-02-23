---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute, quantum program

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Create a quantum program
{: #create-program}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This tutorial walks you through the steps to create a quantum runtime program to run a job on an IBM quantum computer.
{: shortdesc}

A runtime program is a piece of Python code that lives in the cloud and can be invoked by yourself other authorized users (if it is made public).

Create the Python program by following this template. It can also be found in the [qiskit-ibmq-provider repository.](https://github.com/Qiskit/qiskit-ibmq-provider/blob/master/qiskit/providers/ibmq/runtime/program/program_template.py){: external}  The `main()` method is the entry point of a runtime program. It takes backend and user_messenger parameters (injected for you by the system) that can be used to send circuits to the backend and messages to the user, respectively. Any other custom parameters specified when you run the program are injected as kwargs.

 ```Python
"""Runtime program template.

The ``main()`` method is the entry point of a runtime program. It takes
backend and user_messenger parameters (injected for you by the system)
that can be used to send circuits to the backend and messages to the user,
respectively. Any other custom parameters specified when you run the program
are injected as kwargs.
"""

def program(backend, user_messenger, **kwargs):
   """Function that does classical-quantum calculation."""
   # UserMessenger can be used to publish interim results.
   user_messenger.publish("This is an interim result.")
   return "final result"

def main(backend, user_messenger, **kwargs):
   """This is the main entry point of a runtime program.

   The name of this method must not change. It also must have ``backend``
   and ``user_messenger`` as the first two positional arguments.

   Args:
       backend: Backend for the circuits to run on.
       user_messenger: Used to communicate with the program user.
       kwargs: User inputs.

   Returns:
       The final result of the runtime program.
   """
   # Massage the input if necessary.
   result = program(backend, user_messenger, **kwargs)
   # Final results can be directly returned
   return result  
```

 {{site.data.keyword.quantum_long_notm}} uses the latest version of Qiskit.
{: note}

 Each runtime program must have a main() function, which serves as the entry point to the program. This function must have user_messenger as the first positional argument:

user_messenger is an instance of UserMessenger and has a publish() method that can be used to send interim and final results to the program user. This method takes a parameter final that indicates whether it's a final result. However, it is recommended to return the final result directly from the main() function. Currently, only final results are stored after a program execution finishes.


### Example
{: #example-runtime-program-create-program}

 ```Python
"""A sample runtime program that submits random circuits for user-specified iterations."""

import random
from typing import Any

from qiskit import transpile
from qiskit.circuit.random import random_circuit


def prepare_circuits(backend):
    """Generate a random circuit.

    Args:
        backend: Backend used for transpilation.

    Returns:
        qiskit.QuantumCircuit: Generated circuit.
    """
    circuit = random_circuit(num_qubits=5, depth=4, measure=True, seed=random.randint(0, 1000))
    return transpile(circuit, backend)


def main(backend, user_messenger, **kwargs) -> Any:
    """Main entry point of the program.

    Args:
        backend: Backend to submit the circuits to.
        user_messenger: Used to communicate with the
            program consumer.
        kwargs: User inputs.

    Returns:
        Final result of the program.
    """
    iterations = kwargs.pop("iterations", 5)
    for it in range(iterations):
        qc = prepare_circuits(backend)
        result = backend.run(qc).result()
        user_messenger.publish({"iteration": it, "counts": result.get_counts()})

    return "All done!"
  ```


## Next steps
{: #next-steps}

Learn how to [upload a program](/docs/quantum-computing?topic=quantum-computing-program).

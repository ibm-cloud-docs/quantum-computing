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

This product is currently only available internally.  Get notified about {{site.data.keyword.quantum_long_notm}} public experimental release by filling out [this form](https://airtable.com/shrRpebS4aD3XeDhA){: external}.
{: note}

This tutorial walks you through the steps to create a quantum runtime program to run a job on an IBM quantum computer.
{: shortdesc}

## Before you begin
{: #before-create-pgm}

Install and import Qiskit by running `#pip install qiskit`.  If you need further details, you can follow the instructions in the [Qiskit textbook.](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. You need to keep this package updated.

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

There are several runtime programs in the qiskit_runtime directory in this repository. qiskit_runtime/sample_program/sample_program.py is one of them. It is a sample runtime program that submits random circuits for user-specified iterations:

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


## Data serialization
{: #data-serialization}

 Runtime programs live in the cloud, and JSON is the standard way of passing data to and from cloud services. Therefore, when a user invokes a runtime program, the input parameters must first be serialized into the JSON format and then deserialized once received by the server. By default, this process is done automatically by using the RuntimeEncoder and RuntimeDecoder classes.

### Custom classes
{: #custom-classes-create-pgm}

RuntimeEncoder and RuntimeDecoder only support types that are commonly used in Qiskit, such as complex numbers and numpy arrays. These methods have only partial support for using custom Python classes for input or output.

Your custom class must have the following methods:

* A to_json() method that returns a JSON string representation of the object
* A from_json() class method that accepts a JSON string and returns the corresponding object

When RuntimeEncoder serializes a Python object, it checks whether the object has a to_json() method. If so, it calls the method to serialize the object. However, RuntimeDecoder does not invoke from_json() to convert the data back because it doesn't know how to import your custom class. Therefore, the deserialization needs to be done explicitly.

Following is an example of serializing and deserializing a custom class. First, define the class MyCustomClass:

```Python
  import json

  class MyCustomClass:

    def __init__(self, foo, bar):
        self._foo = foo
        self._bar = bar

    def to_json(self):
        """Convert this instance to a JSON string."""
        return json.dumps({"foo": self._foo, "bar": self._bar})

    @classmethod
    def from_json(cls, json_str):
        """Return a MyCustomClass instance based on the input JSON string."""
        return cls(**json.loads(json_str))
```

Note that it has the to_json() method that converts a MyCustomClass instance to a JSON string, and a from_json() class method that converts a JSON string back to a MyCustomClass instance.

The following example shows how you would use MyCustomClass as an input to your program:

```python
  program_inputs = {
    'my_obj': MyCustomClass("my foo", "my bar")
  }

  options = {"backend_name": "ibmq_qasm_simulator"}
                          )
```

Your program can then use the from_json() method to restore the JSON string back to a MyCustomClass instance:

```Python
  def main(backend, user_messenger, **kwargs):
     """Main entry point of the program."""
     my_obj_str = kwargs.pop('my_obj')
     my_obj = MyCustomClass.from_json(my_obj_str)
```

Similarly, if you pass a MyCustomClass instance as an output of your program, it is automatically converted to a JSON string (by using the to_json() method):

```python
 def main(backend, user_messenger, **kwargs):
    """Main entry point of the program."""
    return MyCustomClass("this foo", "that bar")
```

When the user of this program calls job.result(), they receive a JSON string rather than a MyCustomClass instance. The user can convert the string back to MyCustomClass themselves:

```python
  output_str = job.result()
  output = MyCustomClass.from_json(output_str)
```

Alternatively, you can provide a decoder for the users. Your decoder class should inherit ResultDecoder and overwrites the decode() method:

```python
  from qiskit.providers.ibmq.runtime import ResultDecoder

  class MyResultDecoder(ResultDecoder):

    @classmethod
    def decode(cls, data):
        data = super().decoded(data)  # Perform any preprocessing.
        return MyCustomClass.from_json(data)
```

Your user can then use this MyResultDecoder to decode the result of your program:

```python
  output = job.result(decoder=MyResultDecoder)
```

## Testing your runtime program
{: #testing}

You can test your runtime program by using a local simulator before you upload it. Simply import and invoke the main() function of your program and pass in the following parameters:

* The backend instance to use
* A new UserMessenger instance
* Program input parameters that are serialized and then deserialized by the correct encoder and decoder. While this might seem redundant, it ensures that input parameters can be passed to your program properly once it is uploaded to the cloud.

The following example tests the sample-program program previously described. It uses the qasm_simulator from Qiskit Aer as the test backend. It serializes and deserializes input data by using RuntimeEncoder and RuntimeDecoder, which are the default methods used by runtime.

```python

  from qiskit_runtime.sample_program import sample_program
  from qiskit import Aer
  from qiskit.providers.ibmq.runtime.utils import RuntimeEncoder, RuntimeDecoder
  from qiskit.providers.ibmq.runtime import UserMessenger

  inputs = {"iterations": 3}

  backend = Aer.get_backend('qasm_simulator')
  serialized_inputs = json.dumps(inputs, cls=RuntimeEncoder)
  deserialized_inputs = json.loads(serialized_inputs, cls=RuntimeDecoder)

  sample_program.main(backend, user_messenger, **deserialized_inputs)
```


## Defining program metadata
{: #metadata}


Program metadata helps users to understand how to use your program. It includes the following information:

* maximum execution time: The maximum amount of time, in seconds, a program can run before it is forcibly terminated
* description: Describes the program
* version: Program version
* backend_requirements: Describes the backend attributes that are needed to run the program
* parameters: Describes the program input parameters
* return_values: Describes the return values
* interim_results: Describes the interim results

When you upload a program, the only requirement is that you specify the name. Other values are not required but are encouraged. You can specify the metadata fields individually, or use a JSON file.

Following is the metadata JSON file of the sample-program program as an example:

```json
  import os

  sample_program_json = os.path.join(os.getcwd(), "../qiskit_runtime/sample_program/sample_program.json")

  with open(sample_program_json, 'r') as file:
    data = file.read()

  print(data)
```

```json
{
    "name": "sample-program",
    "description": "A sample runtime program.",
    "max_execution_time": 300,
    "spec":
    {
        "backend_requirements":
        {
            "min_num_qubits": 5
        },
        "parameters":
        {
            "$schema": "https://json-schema.org/draft/2019-09/schema",
            "properties":
            {
                "iterations":
                {
                    "type": "integer",
                    "minimum": 0,
                    "description": "Number of iterations to run. Each iteration generates a runs a random circuit."
                }
            },
            "required":
            [
                "iterations"
            ]
        },
        "return_values":
        {
            "$schema": "https://json-schema.org/draft/2019-09/schema",
            "description": "A string that says 'All done!'.",
            "type": "string"
        },
        "interim_results":
        {
            "$schema": "https://json-schema.org/draft/2019-09/schema",
            "properties":
            {
                "iteration":
                {
                    "type": "integer",
                    "description": "Iteration number."
                },
                "counts":
                {
                    "description": "Histogram data of the circuit result.",
                    "type": "object"
                }
            }
        }
    }
}
```

Note that the return value has "name": "-" because there is only one return value. If the program has multiple return values, specify a meaningful name for each.



## Next steps
{: #next-steps}

Learn how to [upload a program](/docs/quantum-computing?topic=quantum-computing-program).
Learn how to [submit a job](/docs/quantum-computing?topic=quantum-computing-run_job).

---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-22"

keywords: quantum, Qiskit, runtime, near time compute, primitive programs, IBM Quantum Platform

subcollection: quantum-computing


---


{{site.data.keyword.attribute-definition-list}}

# Algorithm tuning options
{: #migrate-tuning}

One of the advantages of the primitives is that they abstract away the
circuit execution setup so that algorithm developers can focus on the
pure algorithmic components. However, sometimes, to get the most out of
an algorithm, you might want to tune certain primitive options. This
section describes some of the common settings you might need.
{: shortdesc}

This section focuses on Qiskit Runtime primitive
`.Options` (imported from `qiskit_ibm_runtime`). While most of the `primitives`
interface is common across implementations, most
`.Options` are not. Consult the
corresponding API references for information about the
`qiskit.primitives` and `qiskit_aer.primitives` options.
{: important}

## Shots
{: #migrate-shots}

For some algorithms, setting a specific number of shots is a core part
of their routines. Previously, shots could be set during the call to
`backend.run()`. For example, `backend.run(shots=1024)`.
Now, that setting is part of the execution options (`"second level
option"`). This can be done during the primitive setup:

``` python
from qiskit_ibm_runtime import Estimator, Options

options = Options()
options.execution.shots = 1024

estimator = Estimator(session=session, options=options)
```
{: codeblock}

If you need to modify the number of shots set between iterations
(primitive calls), you can set the shots directly in the `run()` method.
This overwrites the initial `shots` setting.

``` python
from qiskit_ibm_runtime import Estimator

estimator = Estimator(session=session)

estimator.run(circuits=circuits, observables=observables, shots=50)

# other logic

estimator.run(circuits=circuits, observables=observables, shots=100)
```
{: codeblock}

For more information about the primitive options, refer to the [Options class API reference](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/stubs/qiskit_ibm_runtime.options.Options.html#qiskit_ibm_runtime.options.Options){: external}.

## Transpilation
{: #migrate-transpilation}

By default, the Qiskit Runtime primitives perform circuit transpilation.
There are several optimization levels you can choose from. These levels
affect the transpilation strategy and might include additional error
suppression mechanisms. Level 0 only involves basic transpilation. To
learn about each optimization level, view the Optimization level table
in the [Error suppression topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/locale/es_UN/how_to/error-suppression.html#setting-the-optimization-level){: external}.

The optimization level option is a first level option, and can be
set as follows:

``` python
from qiskit_ibm_runtime import Estimator, Options

options = Options(optimization_level=2)

# or..
options = Options()
options.optimization_level = 2

estimator = Estimator(session=session, options=options)
```
{: codeblock}

You might want to configure your transpilation strategy further, and for
this, there are advanced transpilation options you can set up. These are
second level options, and can be set as follows:

``` python
from qiskit_ibm_runtime import Estimator, Options

options = Options()
options.transpilation.initial_layout = ...
options.transpilation.routing_method = ...

estimator = Estimator(session=session, options=options)
```
{: codeblock}

For more information, and a complete list of advanced transpilation
options, see the Advanced transpilation options table in the [Error supppression topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/locale/es_UN/how_to/error-suppression.html#advanced-transpilation-options){: external}.

Finally, you might want to specify settings that are not available
through the primitives interface, or use custom transpiler passes. In
these cases, you can set `skip_transpilation=True` to submit
user-transpiled circuits. To learn how this is done, refer to the
[Submitting user-transpiled circuits using primitives tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/user-transpiled-circuits.html){: external}.

The `skip_transpilation` option is an advanced transpilation option, set
as follows:

``` python
from qiskit_ibm_runtime import Estimator, Options

options = Options()
options.transpilation.skip_transpilation = True

estimator = Estimator(session=session, options=options)
```
{: codeblock}

## Error mitigation
{: #tuning-error-mit}

You might want to leverage different error mitigation methods and see
how these affect the performance of your algorithm. These can also be
set through the `resilience_level` option. The method selected for each
level is different for `Sampler` and `Estimator`. You can find more
information in the [Configure error mitigation topic](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/how_to/error-mitigation.html){: external}.

The configuration is similar to the other options:

``` python
from qiskit_ibm_runtime import Estimator, Options

options = Options(resilience_level = 2)

# or...

options = Options()
options.resilience_level = 2

estimator = Estimator(session=session, options=options)
```
{: codeblock}

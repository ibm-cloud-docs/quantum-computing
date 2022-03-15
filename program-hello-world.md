---

copyright:
  years: 2021, 2022
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: reference
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}


# Hello World program
{: #program-hello-world}

Run the Hello World program to ensure that your environment is set up properly.
{: shortdesc}

The maximum execution time is 600 seconds (10 minutes).

Input:

```Python
from qiskit.test.reference_circuits import ReferenceCircuits
from qiskit_ibm_runtime import IBMRuntimeService

service = IBMRuntimeService()
program_inputs = {'iterations': 1}
options = {"backend_name": "ibmq_qasm_simulator"}
job = service.run(program_id="hello-world",
                  options=options,
                  inputs=program_inputs
                 )
print(f"job id: {job.job_id}")
result = job.result()
print(result)
```
{: codeblock}

Result:

```text
All done!
```

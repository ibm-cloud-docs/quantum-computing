---

copyright:
  years: 2021, 2022
lastupdated: "2022-04-01"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Hello World program
{: #program-hello-world}

Run the Hello World program to ensure that your environment is set up properly.
{: shortdesc}

The maximum execution time is 600 seconds (10 minutes).

Input:

```Python
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

You can view the code for this program in the [Qiskit repository]([https://github.com/Qiskit/qiskit-ibm-runtime/tree/main/program_source/hello_world).

## Next steps
{: #next-steps}

- Use the [Getting started guide](/docs/quantum-computing?topic=quantum-computing-quickstart) to create an instance and run your first job.
- Review [Get started with the Sampler primitive](/docs/quantum-computing?topic=quantum-computing-example-estimator) for a step-by-step example of using this primitive.
- Use Qiskit [tutorials](https://qiskit.org/documentation/tutorials.html){: external} to learn how to create circuits with Qiskit.

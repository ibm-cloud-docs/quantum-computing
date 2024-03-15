---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-29"



keywords: quantum, Qiskit, runtime, near time compute, retired

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Retired systems
{: #retired}

The following systems have been retired. For the list of currently available systems, see the [Compute resources page.](https://cloud.ibm.com/quantum/resources/systems){: external} 


| System name       | Qubit count | Retirement date (Year - month - day) |
| ----------------- | ----------- | --------------- |
| ibm_canberra      | **27**      | 2023-08-15      |
| ibm_nazca         | **127**     | 2023-08-15      |
| ibm_algiers       | **27**      | 2024-02-20      |
{: caption="Retired systems" caption-side="bottom"}


## Retrieve a job from a retired system
{: #retired-retrieve}

To retrieve jobs from a retired system, use code similar to this:

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Load your IBM Quantum account. 
service = QiskitRuntimeService(channel="ibm_cloud", instance="instance_name")

# Retrieve a single job by ID
job = service.job(<job_id>)

# Retrieve a batch of jobs. Filtering options can be found in the QiskitRuntimeService.jobs api reference
jobs = service.jobs(backend_name=<backend_name>)
```

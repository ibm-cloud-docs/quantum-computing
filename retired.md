---

copyright:
  years: 2022, 2023
lastupdated: "2023-12-04"



keywords: quantum, Qiskit, runtime, near time compute, retired

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Retired systems
{: #retired}

The following systems have been retired. For the list of currently available systems, see the [Compute resources page.](https://cloud.ibm.com/quantum/resources/systems){: external} 

To retrieve jobs from a retired system, see [these instructions.](#retired-retrieve)
{: note}


| System name       | Qubit count | Retirement date (Year - month - day) |
| ----------------- | ----------- | --------------- |
| ibm_canberra      | **27**      | 2023-11-28      |
| ibm_nazca         | **127**     | 2023-11-28      |


## Retrieve a job from a retired system
{: #retired-retrieve}

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Load your IBM Quantum account(s). Replace "hub/group/project" with your desired instance
service = QiskitRuntimeService(channel="ibm_quantum", instance="hub/group/project")

# Retrieve a single job by id
job = service.job(<job_id>)

# Retrieve a batch of jobs. Filtering options can be found in the QiskitRuntimeService.jobs api reference
jobs = service.jobs(backend_name=<backend_name>)
```
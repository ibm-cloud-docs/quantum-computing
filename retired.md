---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-29"



keywords: quantum, Qiskit, runtime, near time compute, retired

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Retired QPUs
{: #retired}

The following QPUs have been retired. For the list of currently available QPUs, see the [Compute resources page.](https://cloud.ibm.com/quantum/resources/systems){: external}


The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}


| QPU name       | Qubit count | Retirement date (Year - month - day) |
| ----------------- | ----------- | --------------- |
| ibm_kyoto         | **127**     | 2024-09-05      |
| ibm_osaka         | **127**     | 2024-08-13      |
| ibm_algiers       | **27**      | 2024-02-20      |
| ibm_canberra      | **27**      | 2023-08-15      |
| ibm_nazca         | **127**     | 2023-08-15      |
{: caption="Table: Retired QPUs" caption-side="bottom"}


## Retrieve a job from a retired QPU
{: #retired-retrieve}

To retrieve jobs from a retired QPU, use code similar to this:

```python
from qiskit_ibm_runtime import QiskitRuntimeService

# Load your IBM Quantum account.
service = QiskitRuntimeService(channel="ibm_cloud", instance="instance_name")

# Retrieve a single job by ID
job = service.job(<job_id>)

# Retrieve a batch of jobs. Filtering options can be found in the QiskitRuntimeService.jobs api reference
jobs = service.jobs(backend_name=<backend_name>)
```
{: codeblock}

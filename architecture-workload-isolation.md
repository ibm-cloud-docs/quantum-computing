---

copyright:
  years: 2022, 2023
lastupdated: "2022-03-28"

keywords: public isolation for Qiskit Runtime, compute isolation for Qiskit Runtime, Qiskit Runtime architecture, workload isolation in Qiskit Runtime

---

{{site.data.keyword.attribute-definition-list}}

# Learning about Qiskit Runtime architecture and workload isolation
{: #compute-isolation-runtime}

Qiskit Runtime jobs run in individual containers in an internal Kubernetes cluster to isolate jobs from any other activities of other users. Jobs are not shared or visible between service instances. However, all users that can access a service instance can see that instance’s jobs, or submit jobs the account owner might be charged for.
{: shortdesc}

The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}

## Restricting Access to service instances
{: #workload-isolation-runtime}

With Qiskit Runtime, you can create service instances that are IAM-managed resources. Accordingly, IAM-based access control can be used for these service instances.
User access to Qiskit Runtime service instances can be configured through different mechanisms:
-	Resource groups can be used to group service instances. This lets you manage access permissions based on resource group assignment.
-	Access groups can be used to assign access to individual service instances. Service IDs (with their API keys) can be assigned to these access groups.
-	IAM tags can be used to categorize service instances and use these tags through access groups.

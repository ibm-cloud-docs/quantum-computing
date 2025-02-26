---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-16"

keywords: data encryption in Qiskit Runtime, data storage for Qiskit Runtime, personal data in Qiskit Runtime, data deletion for Qiskit Runtime, data in Qiskit Runtime, data security in Qiskit Runtime,

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in Qiskit Runtime
{: #mng-data}

To ensure that you can securely manage your data when you use Qiskit Runtime, it is important to know exactly what data is stored and encrypted and how you can delete any stored data.
{: shortdesc}

The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}

## Protecting your sensitive data in Qiskit Runtime
{: #data-encryption}

The data that you store in {{site.data.keyword.cloud_notm}} is encrypted at rest by using a randomly generated key.

## Deleting your data in Qiskit Runtime
{: #data-delete}

Deleting a service instance removes all of the content associated with that instance, such as your jobs, results, parameters, and programs. To delete an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances), find the instance you want to remove, click its overflow menu, then click **Delete**. You will be asked to confirm the deletion.

### Deleting Qiskit Runtime instances
{: #service-delete}

The Qiskit Runtime data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the Qiskit Runtime service description, which you can find in the [{{site.data.keyword.cloud_notm}} Terms](https://www.ibm.com/support/customer/csol/terms/?id=i126-9425&lc=en#detail-document){: external}.

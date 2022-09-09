---

copyright:
  years: 2021, 2022
lastupdated: "2022-09-07"


keywords: Qiskit Runtime backend, Qiskit Runtime device, Qiskit Runtime simulator, Qiskit Runtime systems

subcollection: quantum-computing

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}


# Choose a system or simulator
{: #choose-backend}

Before you run a job, you can optionally choose a physical quantum system or a simulator to run on. If you do not specify one, the job is sent to the least busy device that you have access to.
{: shortdesc}

The Standard plan allows access only to physical quantum systems, while the Lite plan allows access only to simulators.
{: #note}

To find your available systems and simulators, view the [Compute resources page](https://cloud.ibm.com/quantum/resources){: external}. You must be logged in to see your available compute resources.

You can also view your available systems and simulators by using the [List your backends](/apidocs/quantum-computing#list-backends){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/list-backends){: external}.

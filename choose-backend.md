---

copyright:
  years: 2021, 2023
lastupdated: "2022-09-07"


keywords: Qiskit Runtime backend, Qiskit Runtime device, Qiskit Runtime simulator, Qiskit Runtime QPUs

subcollection: quantum-computing

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}


# Choose a QPU or simulator
{: #choose-backend}

Before you run a job, you must choose a physical QPU (quantum processing unit) or a simulator to run on. Alternatively, you can use [Qiskit Runtime local testing mode.](https://docs.quantum.ibm.com/guides/local-testing-mode){: external} to run jobs on your local computer.
{: shortdesc}

The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}

The Standard plan allows access to both physical QPUs and simulators, while the Lite plan (deprecated) allows access only to simulators.
{: note}

To find your available QPUs and cloud simulators, view the [Compute resources page](https://cloud.ibm.com/quantum/resources){: external}. You must be logged in to see your available compute resources. 

You can also view your available QPUs and simulators by using the [List your backends](/apidocs/quantum-computing#list-backends){: external} API directly or in [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Programs/list-backends){: external}.

---

copyright:
  years: 2021, 2023
lastupdated: "2022-09-07"


keywords: Qiskit Runtime cost, Qiskit Runtime instance, Qiskit Runtime job cost, Qiskit Runtime price

subcollection: quantum-computing

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}


# View the cost
{: #view-cost}

At any time, you can see how much cost has been incurred by jobs associated with an instance.
{: shortdesc}

## View instance cost
{: #view-instance-cost}

To determine how much has been billed to an instance during the current billing cycle, from the [Instances page](https://cloud.ibm.com/quantum/instances), click the instance to open its details page.

These are the fields relevant to cost:

- **Billing cycle usage**: Qiskit Runtime usage by this instance during the current billing cycle. This usage is the time counted by Qiskit Runtime to process a job, and is determined by the use of internal resources.
- **Billing cycle cost**: The total cost of running jobs during the current billing cycle.
- **Total usage**: Qiskit Runtime usage by this instance since it was created.
- **Total cost**: The total cost of running jobs on this instance since it was created. Only administrators can set this value.

You can view your billing cycle on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing).

## View job cost
{: #view-job-cost}

To determine how much has been billed to each job associated with an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances), click the instance to open its details page. Next, on the left side, click Jobs.

These are the columns relevant to cost:

- **Usage**: Qiskit Runtime used by this job. This usage is the time counted by Qiskit Runtime to process a job, and is determined by the use of internal resources.
- **Cost**: The total cost of running this job.

## Next steps
{: #next-steps-view-cost}

To learn about how costs are incurred, see [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-plans).

To learn how to limit costs, see [Manage costs](/docs/quantum-computing?topic=quantum-computing-cost).

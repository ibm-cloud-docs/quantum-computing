---

copyright:
  years: 2021, 2022
lastupdated: "2022-09-29"


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

To determine how much has been billed to an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances){: external}, click the instance to open its details page, then find the **Instance usage** section.

These are the fields relevant to cost:

* **Billing cycle QR usage**: The amount of Qiskit Runtime resources used by this instance during the current billing cycle. This includes both quantum and classical resource usage.  
* **Billing cycle cost**: The total cost of running jobs on this instance during the current billing cycle.
* **Total QR usage (all time)**: Amount of Qiskit Runtime resources used since this instance was created. This includes both quantum and classical resource usage.  
* **Total cost (all time)**: The total cost of running jobs on this instance since it was created.

You can view your billing cycle on the [Billing and usage page](https://cloud.ibm.com/billing){: external}.

## View job cost
{: #view-job-cost}

To determine how much has been billed to each job associated with an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances){: external}, click the instance to open its details page. Next, on the left side, click Jobs.

These are the columns relevant to cost:

* **QR usage**: The amount of Qiskit Runtime resources (both quantum and classical) used by this job
* **Cost**: The total cost of running this job

## Next steps
{: #next-steps-view-cost}

To learn about how costs are incurred, see [Pricing overview](/docs/quantum-computing?topic=quantum-computing-cost#pricing-overview).

To learn how to limit costs, see [How to limit your cost](/docs/quantum-computing?topic=quantum-computing-cost#limit-cost).

---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-24"

keywords: quantum, Qiskit, runtime, near time compute, standard plan, pay-as-you-go, lite plan

subcollection: quantum-computing

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}

# Manage costs
{: #cost}

The Standard plan is not free. Use the information in this topic to help you understand how much you’re paying and how to limit your costs.  The described fields do not exist for instances in the Lite plan (deprecated).
{: shortdesc}

The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}

## How to limit your cost
{: #limit-cost}

There are several ways to limit your costs.

### Minimize iterations and shots
{: #min-shots}

The time your job takes (and therefore, its cost) depends on how many iterations you make in a session and how many shots are run in each iteration. Thus, you can manage your cost by running only as many iterations and shots as you need. Additionally, sessions run in dedicated mode.  That is, your session jobs get exclusive access to the backend and you are charged for the entire time the session is open, starting when the first session job starts being processed. For information, refer to the [Execution modes](https://quantum.cloud.ibm.com/guides/execution-modes){: external} documentation.

### Set time limits
{: #time-limits}

The maximum execution time for a Qiskit Runtime workload is the smallest of these values:

* The `max_execution_time` defined on the job's options by using the ``max_execution_time`` parameter. This execution time is the time that the QPU complex (including control software, control electronics, QPU, and so on) is engaged in processing the job.
* The service-calculated limit - The service calculates an appropriate job timeout value based on the input circuits and options. This timeout is capped at 3 hours to ensure fair device usage.

For example, if you specify `max_execution_time=5000` (approximately 83 minutes), but the service determines it should not take more than 300 seconds (5 minutes) to execute the job, then the job is cancelled after 5 minutes.

If you are using sessions or batches, you can also set the session's or batch's `max_time` parameter (in wall clock time).

To keep sessions and batches from surpassing the cost limit, the system will override their `max_time` settings if necessary.
{: important}

You must use CLI or the API to work with these settings. For instructions, see the [Maximum execution time for Qiskit Runtime workloads](https://quantum.cloud.ibm.com/docs/guides/manage-cost){: external} topic.

### Set the cost limit
{: #admin-limit-cost}

An instance administrator can limit how much is spent. For instructions, see the [Maximum execution time for Qiskit Runtime workloads](https://quantum.cloud.ibm.com/docs/guides/manage-cost){: external} topic.

The instance's cost limit refers to the total cost of all jobs run with this instance since it was created, and it will always be greater than or equal to the Total cost. After the instance reaches the specified limit, no further jobs can be run and no more cost is incurred.

The cost limit is always specified in US dollars (USD), then converted to runtime seconds.  However, for monthly billing purposes, you are charged in your local currency, specified on your IBM Cloud account. Because currency exchange rates can fluctuate, the cost for _X_ runtime seconds might be different when initially calculated in USD than when you're actually charged in your local currency.  As a result, if your local currency is not USD, the total amount charged for the number of seconds specified in this field could vary from the dollar amount you specify.
{: note}

## How to remove a cost limit
{: #unlimit-cost}

An instance administrator can remove the cost limit. For instructions, see the [Maximum execution time for Qiskit Runtime workloads](https://quantum.cloud.ibm.com/docs/guides/manage-cost){: external} topic.

### What happens when the cost limit is reached
{: #cost-limit-reached}

When the instance's cost limit is reached, the currently running job is stopped.  Its status is set to `Cancelled` with a reason of `Ran too long`. Any available partial results are kept.

No further jobs can be submitted by using this instance until the cost limit is increased.

## How to see what you're being charged
{: #pricing-bill}

You are sent a monthly invoice that provides details about your resource charges. You can check how much has been spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing){: external}.

Additionally, you can determine cost per instance or per job at any time.

## Set up spending notifications
{: #pricing-notifications}

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account **Type** description](/docs/account?topic=account-accounts){: external}.

- The notifications trigger only _after_ cost surpasses the specified limit.
- Cost is submitted to the billing system hourly. Thus, a long delay might occur between the job submission and the spending notification being sent.
- The billing system can take multiple days to get information to the invoicing system, which might cause further delay in notifications. For more information about how the IBM Cloud billing system works, see [Setting spending notifications](/docs/account?topic=account-spending).

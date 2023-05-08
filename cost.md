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

The Standard plan is not free. Use the information in this topic to help you understand how much you’re paying and how to limit your costs.
{: shortdesc}

## Time limits
{: #time-limits}

There are time limits on programs, systems, and sessions.

* **Primitives:** The maximum execution time for the Sampler primitive is 10000 seconds (2.78 hours). The maximum execution time for the Estimator primitive is 18000 seconds (5 hours).
* **Programs:** The maximum execution time for a program is set on the job's options by using the ``max_execution_time`` parameter. If specified, it must be at least 300 seconds. 
* **Prototype programs:** The maximum execution time is listed on the [Prototype programs page](https://quantum-computing.ibm.com/services/programs/prototypes).
* **Systems:** The system limit on the job execution time is 3 hours for a job that is running on a simulator and 8 hours for a job running on a physical system.
* **Sessions:** When a session is started, it is assigned a maximum session timeout value, which is the system limit by default.  After the maximum session timeout is reached, the session is permanently closed. The maximum session timeout value is set on the ``max_time`` parameter, which can be greater than the program’s ``max_execution_time``.

  Additionally, there is a 5 minute *interactive* timeout value. If there are no session jobs queued within that window, the session is temporarily deactivated and normal job selection resumes. After the new session becomes inactive, if the job scheduler gets a job from the original session and its maximum timeout value has not been reached, the session is reactivated until its maximum timeout value is reached.
  
  The timer for ``max_time`` is not paused during any temporary deactivation periods due to interactive timeouts.
  {: note}

## How to limit your cost
{: #limit-cost}

The time your job takes (and therefore, its cost) depends on how many iterations you make in a session and how many shots are run in each iteration. Thus, you can manage your cost by running only as many iterations and shots as you need.

Additionally, an instance administrator can limit how much is spent. To set cost limits, navigate to the [IBM Cloud Instances page](https://cloud.ibm.com/quantum/instances) then click the instance and set the **Cost limit**.

The instance's cost limit refers to the total cost of all jobs run with this instance since it was created, and it will always be greater than or equal to the Total cost. After the instance reaches the specified limit, no further jobs can be run and no more cost is incurred.

The cost limit is always specified in US dollars (USD), then converted to runtime seconds.  However, for monthly billing purposes, you are charged in your local currency, specified on your IBM Cloud account. Because currency exchange rates can fluctuate, the cost for `X` runtime seconds might be different when initially calculated in USD than when you're actually charged in your local currency.  As a result, if your local currency is not USD, the total amount charged for the number of seconds specified in this field could vary from the dollar amount you specify.
{: note}

## How to remove a cost limit
{: #unlimit-cost}

An instance administrator can remove the cost limit.  To do so, navigate to the [IBM Cloud Instances page](https://cloud.ibm.com/quantum/instances), then open the instance and click the edit button by the **Cost limit**. Delete the value and click **Save**.

### What happens when the cost limit is reached
{: #cost-limit-reached}

When the instance's cost limit is reached, the currently running job is stopped.  Its status is set to `Cancelled` with a reason of `Ran too long`. Any available partial results are kept. 

No further jobs can be submitted by using this instance until the cost limit is increased. 

## How to see what you're being charged
{: #pricing-bill}

You are sent a monthly invoice that provides details about your resource charges. You can check how much has been spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing).

Additionally, you can determine cost per instance or per job at any time.

### View instance cost
{: #view-instance-cost}

To determine how much has been billed to an instance during the current billing cycle, from the [Instances page](https://cloud.ibm.com/quantum/instances), click the instance to open its details page.

These are the fields relevant to cost:

- **Billing cycle QR usage**: The amount of quantum runtime used by this instance during the current billing cycle
- **Billing cycle cost**: The total cost of running jobs during the current billing cycle
- **Total QR usage**: The amount of quantum runtime used by this instance since it was created
- **Total cost**: The total cost of running jobs on this instance since it was created

You can view your billing cycle on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing).

### View job cost
{: #view-job-cost}

To determine how much has been billed to each job associated with an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances), click the instance to open its details page. Next, on the left side, click Jobs.

These are the columns relevant to cost:

- **QR usage**: The amount of quantum runtime used by this job
- **Cost**: The total cost of running this job

## Set up spending notifications
{: #pricing-notifications}

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account **Type** description](/docs/account?topic=account-accounts){: external}.

- The notifications trigger only _after_ cost surpasses the specified limit.
- Cost is submitted to the billing system hourly. Thus, a long delay might occur between the job submission and the spending notification being sent.
- The billing system can take multiple days to get information to the invoicing system, which might cause further delay in notifications. For more information about how the IBM Cloud billing system works, see [Setting spending notifications](/docs/billing-usage?topic=billing-usage-spending).

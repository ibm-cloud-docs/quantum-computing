---

copyright:
  years: 2022
lastupdated: "2022-04-05"

keywords: quantum, Qiskit, runtime, near time compute, standard plan, pay-as-you-go

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Qiskit Runtime Standard plan
{: #cost}

Learn how to determine cost when using the Standard plan.
{: shortdesc}

## Pricing plan overview
{: #pricing-overview}

The Standard plan charges you per _runtime second_. The following diagram illustrates how job cost is calculated. For this service, one second is one second of execution time when the Qiskit program is running, whether on a quantum processor or accompanying classical cluster.

![This diagram shows that everything before the program starts (such as queuing) is free.  The Once the job starts, it costs $1.60 per second.](images/Runtime_Accounting_Diagram.png "Runtime second accounting"){: caption="Figure 1. Runtime second accounting" caption-side="bottom"}

## Time limits on programs
{: #time-limits}

The maximum execution time for the Sampler primitive is 10000 seconds (2.78 hours). The maximum execution time for the Estimator primitive is 18000 seconds (5 hours).

## How to limit your cost
{: #limit-cost}

The time your job takes (and therefore, its cost) depends on how many iterations your call has and how many shots are run in each iteration.  Thus, you can limit your cost by running only as many iterations and shots as you need.

One way to determine how many iterations and shots you need is to first run the job on a simulator via an instance on the Lite plan.

## How to see what you're being charged
{: #pricing-bill}

You will receive a monthly invoice that provides details about your resource charges. You can check how much you've spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing).

## Set up spending notifications
{: #pricing-notifications}

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account type decscription](https://cloud.ibm.com/docs/account?topic=account-accounts). Note that {{site.data.keyword.cloud}} spending notifications trigger only _after_ cost has surpassed the specified limit.

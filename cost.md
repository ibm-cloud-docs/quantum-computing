---

copyright:
  years: 2022
lastupdated: "2022-11-15"

keywords: quantum, Qiskit, runtime, near time compute, standard plan, pay-as-you-go, lite plan

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Manage costs
{: #cost}

Follow these tips to manage your cost.
{: shortdesc}


## Time limits on programs
{: #time-limits}

The maximum execution time for the Sampler primitive is 10000 seconds (2.78 hours). The maximum execution time for the Estimator primitive is 18000 seconds (5 hours).

Additionally, the system limit on the job execution time is 3 hours for a job running on a simulator and 8 hours for a job running on a physical system.

## How to limit your cost
{: #limit-cost}

The time your job takes (and therefore, its cost) depends on how many iterations you make in a session and how many shots are run in each iteration. Thus, you can manage your cost by running only as many iterations and shots as you need. As we expand on the primitives interface, we will provide more ways to manage cost. 

## How to see what you're being charged
{: #pricing-bill}

You will receive a monthly invoice that provides details about your resource charges. You can check how much you've spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing).

## Set up spending notifications
{: #pricing-notifications}

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account **Type** description](https://cloud.ibm.com/docs/account?topic=account-accounts). Note that {{site.data.keyword.cloud}} spending notifications trigger only _after_ cost has surpassed the specified limit.

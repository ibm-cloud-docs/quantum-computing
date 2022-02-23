---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# View final job results
{: #view-final}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This tutorial describes how to review final results after running a job. For a detailed example, see the example [here](/docs/quantum-computing?topic=quantum-computing-example).
{: shortdesc}

You can run a job by using Qiskit Runtime (Python) or by directly calling the Quantum Cloud Runtime API (curl).  To view the appropriate information, choose Python or curl below:

```
Leave this tab selected if you use Python.
```
{: codeblock}
{: python}

```
Leave this tab selected if you use curl.
```
{: pre}
{: curl}

## Before you begin
{: #before-you-begin-results}

Run your job and note the job ID.

## Check the status
{: #check-job-status-before-results}

After the job completes, you can view the results.

Immediately after running the job, follow up the Qiskit Runtime IBMRuntimeService.run() method by running `job.status()`.
{: python}

If you ran other jobs since running the job you want to investigate, run `job = service.job(job_id)` then run `job.status()`.
{: python}

Run the [List job details API](/apidocs/quantum-computing#get-job-details-jid){: external}, manually or by using [Swagger](https://us-east.quantum-computing.test.ibm.com/openapi/#/Jobs/get_job_details_jid){: external} to check the job's status.
{: curl}

## View the results
{: #view-results}


Follow up the Qiskit Runtime IBMRuntimeService.run() method by running `job.result()`.
{: python}

Run the [list job results API](/apidocs/quantum-computing#get-job-results-jid){: external} ([Swagger link](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Jobs/get_job_results_jid){: external}).
{: curl}

## Next steps
{: #next-steps}

- View the [API reference](/apidocs/quantum-computing/quantum-computing){: external}.
- Learn about IBM Quantum:
    - [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}
    - [Qiskit](https://qiskit.org/){: external}

---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-07"

keywords: Qiskit Runtime sessions, Qiskit Runtime reservations

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Sessions
{: #sessions}

Jobs can run within a session window. The scheduler prioritizes the jobs belonging to that session. Qiskit Runtime primitives transparently take advantage of other session features like shared caching used by the jobs. This helps primitives run as efficiently as possible in the quantum data center and helps users experience a faster turnaround on results for their workload.

## How to run a job in a session
{: #run_session}

In Qiskit, you can associate a primitive with a session by using a context manager. When you call a primitive by using a context manager and that job runs, a session is started (or the job is run in an active session if a session is already active). For example, the following code uses a context manager to call the Estimator primitive:

```Python
with Estimator(...):
```
{: codeblock}

When you start a runtime job with the jobs API, by default it does not run in a session. You can specify that it should run in a new session (`start_session=true`) or you can assign the job to run in a current session (`session_id=ID of a running session`). For information about using the jobs API, see the [Quiskit Runtime API Reference](https://cloud.ibm.com/apidocs/quantum-computing#create-job){: external}


## How session jobs fit into the job queue
{: #queue_session}

For each backend, the first job in the session waits its turn in the queue normally, but subsequent jobs within the same session take priority over any other queued jobs. If there are no jobs that are part of a session, the next job from the regular fair-share queue is run. Jobs still run one at a time, so jobs belonging to a session may still queue up if you already have one running, but you are not waiting for them to be done before submitting more jobs.


   A job from the fair-share queue could activate or reactivate a session.
   {: note}


## How long is a session active?
{: #active_session}

When a session is started, it is assigned a maximum session timeout value.  This timeout value is the same as the initial job's maximum execution time and is the smaller of 1) the system limit (8 hours for physical systems) and 2) the max_execution_time defined by the program.  After this time limit is reached, the session is permanently closed.

Additionally, there is an interactive timeout value. If there are no session jobs queued within that window, the session is temporarily de-activated and normal fairshare job selection resumes. If the scheduler gets a job from the session again, the session is re-activated until its maximum timeout value is reached.

When using primitives with their context managers as described above, the session is closed automatically when the block is exited.

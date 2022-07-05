---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-05"

keywords: Qiskit Runtime sessions, Qiskit Runtime reservations

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}


# Sessions
{: #sessions}

When you run a job on a quantum system, it can activate a session on that system, it can run in a session that is already active, or it can run outside of a session.  While a session is active, the scheduler prioritizes the jobs in that session.

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

For each backend, jobs that are part of an active session take priority.  If there are no jobs that are part of a session, the next job from the regular fair-share queue is run.


   A job from the fair-share queue could activate or reactivate a session.
   {: note}


## How long is a session active?
{: #active_session}

When a session is started, it is assigned a maximum session timeout value.  This timeout value is the same as the initial job's maximum execution time and is the smaller of 1) the system limit (8 hours for physical systems) and 2) the max_execution_time defined by the program.  After this time limit is reached, the session is permanently closed.

Additionally, there is an interactive timeout value. If there are no session jobs queued within that window, the session is temporarily de-activated and normal fairshare job selection resumes. If the scheduler gets a job from the session again, the session is re-activated until its maximum timeout value is reached.

When using a Qiskit `with` block, the session is closed when the block is exited.

---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute, quantum program

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Get ready to work with your Quantum Service instance
{: #access}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This product is currently only available internally.  Get notified about {{site.data.keyword.quantum_long_notm}} public experimental release by filling out [this form](https://airtable.com/shrRpebS4aD3XeDhA){: external}.
{: note}

This tutorial walks you through the steps to generate the HTTP header that is needed to access your Quantum Service instance and optionally install Qiskit. In order to generate your HTTP header, you need several identifying pieces of information to pass to the instance.
{: shortdesc}

1. Find and copy your API key. From the [{{site.data.keyword.cloud_notm}} API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key. You might need to use light theme to see your entries.  Your key will look something like this: `poJraWQiOiIyMDIxMTAzMDE1MTQiLCJhbGciiuJSUzI1NiJ9.eyJpYW1fdWQiOiJJQk1pZC0xMTAwMDBCS1VWIiwiaWQiOiJJQk1pZC0xMTAwMDBCS1VWIiwicmVhbG1pZCI6IklCTWlkIiwianRpIjoiMjgzMDE3MWItYmY1MC00ZGEyLWE4MjAtMjFmNGVjYWQ0NDE0IiwiaWRlbnRkj`

3. Find your Cloud Resource Name (CRN). From the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external}, scroll to Resource summary, click "Services and software", then click the row that contains your quantum service instance (not the name of the instance). In the pane that opens, click the icon to copy your CRN. For example:
   ```text
      crn:v1:bluemix:public:quantum-computing:us-east:a/b947c1c5a9378d64aed96696e4d76e8e:a3a7f181-35aa-42c8-94d6-7c8ed6e1a94b::
   ```

4. Use the following request to interface with the service:

   ```curl
      curl --location --request GET 'https://us-east.quantum-computing.cloud.ibm.com/programs' \
      --header 'Service-CRN: <crn>'
      --header 'Authorization: apikey <IAM-API-key>'
   ```

5. Optionally install Qiskit: `pip install qiskit`. You need it installed if you are going to modify programs or to work with program results. For more detailed instructions, refer to the [Qiskit textbook.](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. You  need to keep this package updated.



## Next steps
{: #next-steps}

Learn how to [upload a program](/docs/quantum-computing?topic=quantum-computing-program).
Learn how to [submit a job](/docs/quantum-computing?topic=quantum-computing-run_job).

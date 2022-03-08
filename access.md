---

copyright:
  years: 2021, 2022
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

This tutorial walks you through the steps to find your account credentials and authenticate with the service.
{: shortdesc}

You can run a job by using Qiskit Runtime (Python) or by directly calling the Quantum Cloud Runtime API (curl).  The text and code samples update dynamically depending on your choice. Choosing an option in one table updates the content in the entire page. To view the appropriate information, choose Python or curl below:

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

1. Install Qiskit: `pip install qiskit`. You need it installed to create circuits and work with primitive programs via Qiskit Runtime. For detailed instructions, refer to the [Qiskit textbook.](https://qiskit.org/textbook/ch-appendix/qiskit.html){: external}. You need to keep this package updated.

4. Install Qiskit Runtime: `pip install qiskit-ibm-runtime`.

1. Find and copy your API key. From the [{{site.data.keyword.cloud_notm}} API keys page](https://cloud.ibm.com/iam/apikeys){: external}, view or create your API key. Your key will look something like this: `I9sxojrwurPrMWqNR_wc4rztMgVqE1HUmsfACMyw_G9n`

3. **Optional**{: python} Find your Cloud Resource Name (CRN). From the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external}, scroll to Resource summary, click "Services and software", then click the row that contains your quantum service instance (not the name of the instance). In the pane that opens, click the icon to copy your CRN. For example:

```text
      crn:v1:bluemix:public:quantum-computing:us-east:a/b947c1c5a9378d64aed96696e4d76e8e:a3a7f181-35aa-42c8-94d6-7c8ed6e1a94b::
```

5. Call  `IBMRuntimeService` with your IBM Cloud API key and the CRN.

   If using Python, you can use the name of your service instance instead of the CRN.  You can also optionally save your credentials on disk (in the $HOME/.qiskit/qiskit-ibm.json file). By doing so, you only need to use IBMRuntimeService() in the future to initialize your account. If you don't save your credentials to disk, you have to run this command every time you start a new session.

```
   from qiskit_ibm_runtime import IBMRuntimeService

   # Save account to disk.
   IBMRuntimeService.save_account(auth="cloud", token=<IBM Cloud API key>, instance=<IBM Cloud CRN or Service Name>)

   service = IBMRuntimeService()
```
   {: codeblock}
   {: python}

```
   curl --location --request GET 'https://us-east.quantum-computing.cloud.ibm.com/programs' \
   --header 'Service-CRN: <crn>'
   --header 'Authorization: apikey <IAM-API-key>'
```
   {: pre}
   {: curl}

    To update your saved credentials, run `save_account` again, passing in `overwrite=True`  and the updated credentials.  For more information about managing your account see the [account management tutorial](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/04_account_management.html){: external}.
    {: python}

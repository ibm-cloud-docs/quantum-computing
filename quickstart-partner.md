---

copyright:
  years: 2022, 2024
lastupdated: "2024-02-14"

keywords: quantum, Qiskit, runtime, near time compute, channel partner, reseller, 3rd party, vendor, b2b

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}

# Getting started for channel partners
{: #quickstart-partner}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

This tutorial walks through the steps to set up a {{site.data.keyword.qiskit_runtime_notm}} service instance as a channel partner.
{: shortdesc}

## Before you begin
{: #before-partner}

Create one IBM Cloud account from the [registration page](https://cloud.ibm.com/registration){: external}. After creating an account, the IBM Quantum team will grant your service accounts access to the Channel Partner plan. After being notified that you have been granted access, continue to the next step.

## Create a service instance
{: #create-configure-partner}
{: step}

1. From the [{{site.data.keyword.qiskit_runtime_notm}} Provisioning page](/catalog/services/quantum-computing){: external}, choose the Channel Partner service plan.  If you don't see this plan, contact your IBM Quantum representative.

2. In the _Select a location_ dropdown, select a location (`eu-de` if you will opereate from EU).

3. Complete the required information, then click **Create**.

4. Copy the CRN in the _Details panel_, you will need it later.

## Create an access group
{: #crt-access-group}
{: step}

This access group will have the private (and invisible) `ChannelPartnerOperator` role on it.  It is only used for a limited set of  Multi-Channel Scheduler (MCS) service APIs and to connect to the MCS service websocket server.

2. Create an access group.
   1. Go to [Manage → IAM → Access groups](https://cloud.ibm.com/iam/groups){: external} and click **Create**.
   2. Enter a name and a description.
2. Share the access group ID with IBM.  IBM will then manually add the `ChannelPartnerOperator` role.
   1. To find the ID, go to the [Access groups page](https://cloud.ibm.com/iam/groups){: external} then click your access group to open it.
   2. Click the **Details** button, then click the Copy icon to copy the ID.
3. Add the Qiskit Runtime `Manager` role to the access group on the appropriate Qiskit Runtime instance.  This role is necessary for working with the channels runtime API to submit jobs, list backends, and so on.
   1. Go to [Manage → IAM → Access groups](https://cloud.ibm.com/iam/groups){: external}
   2. Select the Access tab and click **Assign access**.
   3. In the Service list, search for **Qiskit Runtime** and select it, then click **Next**.
   4. For Resources, select **All resources**, then click **Next**.
   5. For Roles and actions, select **Manager**. Click **Add**, then **Assign**.


## Create a service ID
{: #crt-service-id}
{: step}

This service ID is similar to a functional user.

1. Create a service ID.
   1. Go to [Manage → IAM → Service IDs](https://cloud.ibm.com/iam/serviceids){: external}  and click **Create**.
   2. Create a name and description and click **Create**.
2. Generate an API key for the service ID. This key, along with the CRN for the Qiskit Runtime service instance created in [step 1](#create-configure-partner), is used to authorize to the channels runtime and MCS service APIs.
   1. Hover over the service ID's row,  click the three-dot Actions icon, then click **Manage service ID**.
   4. Click **API keys** -> **Create**.
   5. Add a name and description, then click **Create**.
2. Add the service ID to the access group that was created in the [previous step](#crt-access-group).
   1. Go to [Manage → IAM → Access groups](https://cloud.ibm.com/iam/groups){: external} and click **Service IDs > Add Service IDs.**
   2. Select the Service ID to add it to the access group.

## Provision a Cloud Object Storage (COS) instance for Bring Your Own Bucket (BYOB)
{: #cos}
{: step}

1. Go to the [Create Cloud Object Storage](https://cloud.ibm.com/objectstorage/create){: external} page and click **IBM Cloud**.  Satellite COS instances are not supported.
2. Give the service instance a name and choose a plan.  The Lite plan is not recommended.
3. Click **Create**.


## Set up to use BYOB
{: #byob-setup}
{: step}

1. Find your COS instance on your [resource list.](https://cloud.ibm.com/resources){: exernal}
2. Open the instance and click **Create Bucket**.
3. Click the arrow under **Customize your bucket**.
4. Fill out the page.  The default options will all work, but you can change anything as required.  Be sure to note your choice for **Resiliency** and **Location** as this will be referenced when using BYOB.
   *Resiliency* refers to the scope and scale of the geographic area across which your data is distributed.

   To maintain the job data in the same region you are located, it is important to choose the **Location** value that reflects your actual area.
   {: note}

   * **Cross Region** resiliency spreads your data across several metropolitan areas.
   * **Regional** resiliency spreads data across a single metropolitan area.
   * A **Single Data Center** only distributes data across devices within a single site. When choosing the location, choose the data center that is in your region.  To determine the locations of the data centers in the list, see [Data centers](/docs/overview?topic=overview-locations#data-centers) on the Region and data center locations for resource deployment page.
5. Go to the Bucket configuration page to obtain your bucket CRN.
6. If appropriate, consider activating [EU support](https://cloud.ibm.com/account/settings) for your account to influence how IBM Cloud handles support tickets for your Cloud Object Storage. This may be of particular interest if you don't use [Bring your own keys](https://cloud.ibm.com/docs/key-protect?topic=key-protect-importing-keys) (BYOK) or [Keep your own key](https://cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-uko-understand-concepts#uko-kyok-concept) (KYOK) encryption. Refer to the [IBM Cloud documentation on EU support](https://cloud.ibm.com/docs/account?topic=account-eu-supported) for more details. Note that EU support applies to your Cloud Object Storage only.  It does not apply to Qiskit Runtime in IBM Cloud support issues.

## Set up a service-to-service authorization policy between Qiskit Runtime and COS
{: #auth-policy}
{: step}

Go to the [Grant a service authorization](https://cloud.ibm.com/iam/authorizations/grant){: external} page and specify these:

* Source Account: This Account
* Source Service: Qiskit Runtime
   * Scope: Resources based on selected attributes
   * Under Add attributes, select Source service instance and specify *your channel partner instance*
* Target Service: Cloud Object Storage
   * Scope: Resources based on selected attributes
   * Under Add attributes, select Service instance and specify *your cos instance*
* Service Access: Object Writer, Content Reader

## Use BYOB
{: #byob-use}
{: step}

To use BYOB, you must provide a `remote_storage` in the `POST /jobs` request body.  See the [IBM Quantum Qiskit Runtime API](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Jobs/create_job){: external} for full details.

```JSON
{
  "program_id": "sampler",
  "backend": "ibm_brussels",
  ...
  "remote_storage": {
    "region": "<region from step 6 in Use BYOB>",
    "region_type": "<regional|cross-region|single-site>",
    "bucket_crn": "<crn of your bucket>"
  }
}
```
{: codeblock}

Region, region type, and bucket CRN can be specified at the root level of `remote_storage` to apply to all job artifacts. For each job artifact below, if you want to use BYOB, add the appropriate key and JSON object with the object name to remote storage. You can specify which job artifacts you want to use BYOB for.

### Job params (required)
{: #byob-use-parms}

Upload your desired `job params` JSON object to your bucket, and give it a name. You will use later in the `POST /jobs`.  Do not delete this object before the job completes.  An example of a `job params` JSON for a Sampler circuit is:

```JSON
{
  "pubs": [[
    "OPENQASM 3.0; include \"stdgates.inc\"; bit[1] c; x $0; c[0] = measure $0;"
  ]],
  "options": {},
  "version": 2
}
```


Once you has uploaded a `job params` JSON object to your bucket, in the `POST /jobs` body, add the following object to your `remote_storage` object under the `job_params` key:

```JSON
{
  "remote_storage": {
    "region": "...",
    ...
    "job_params": {
      "object_name": "name of your 'job params' object in your bucket"
    }
  }
}
```
{: codeblock}

### Results (required)
{: #byob-use-results}

Add the following object to your `remote_storage` object under the `results` key:

```JSON
{
  "remote_storage": {
    ...
    "results": {
      "object_name": "name of your desired results object name"
    }
  }
}
```
{: codeblock}

### Transpiled circuits
{: #byob-use-transpile}

Add the following object to your `remote_storage` object under the `transpiled_circuits` key:

```JSON
{
  "remote_storage": {
    ...
    "transpiled_circuits": {
      "object_name": "name of your desired transpiled circuits object name"
    }
  }
}
```
{: codeblock}

### Logs
{: #byob-use-logs}

Add the following object to your `remote_storage` object under the `logs` key:

```JSON
{
  "remote_storage": {
    ...
    "logs": {
      "object_name": "name of your desired logs object name"
    }
  }
}
```
{: codeblock}

### Full example
{: #full-example-byob}

A full curl example to call `POST /jobs` to send a Sampler primitive to `ibm_brussels` with a bucket located in `eu-de` would be:

```bash
  curl	--request POST \
        --url https://qiskitruntime.eu-de.quantum-computing.cloud.ibm.com/jobs \
        --header 'Authorization: apikey <Service ID apikey>' \
        --header 'Content-Type: application/json' \
        --header 'Service-CRN: <Qiskit Runtime instance CRN>' \
        --data '{
          "program_id": "sampler",
          "backend": "ibm_brussels",
          "remote_storage": {
            "type": "ibmcloud_cos",
            "region": "eu-de",
            "region_type": "regional",
            "bucket_crn": "<Bucket CRN>",
            "results": {
              "object_name": "<results object name>"
            },
            "job_params": {
              "object_name": "<job params object name>"
            },
            "logs": {
              "object_name": "<logs object name>"
            },
            "transpiled_circuits": {
              "object_name": "<transpiled circuits object name>"
            }
          }
        }'
```

where:

- `<Service ID apikey>`: Is the apikey created in the [step 2](#crt-service-id) of the Create a service ID section.
- `<Qiskit Runtime instance CRN>`: The CRN of the [step 4](#create-configure-partner) of the _Create a service instance_ section.
- `<Bucket CRN>`: The CRN of the [step 5](#byob-setup) of the _Set up to use BYOB_ section.
- `<job params object name>`: The name of the JSON object uploaded to the bucket at the [beggining](#byob-use-parms) of this section.
- `<transpiled circuits/results/logs object name>`: The name of the objects that the service will write.


## Integrate into your own channel code
{: #integrate-byob}

This BYOB interface is offered by the backend service and should be called by your channel functionality. Your channel code needs to handle the communication, including authentication, authorization, and data flow between users and your channel. Typical channel functions also include user management, verifying user eligibility to use quantum resources, authentication and authorization, user and tenant isolation of workload against other users, job prioritization and scheduling, offering monitoring, log and audit trail interfaces, as well as potential chargeback integration.

Modifications to `qiskit-ibm-runtime` or your own version of such client code might be required to connect from the Qiskit SDK to your channel component. Qiskit would still generate the workload passed through and on to the primitives.

### Use virtual private endpoints for VPC to connect privately to Qiskit Runtime
{: #vpe}

It is recommended that channel partners integrate by using the virtual private endpoints, when possible. See [Using virtual private endpoints for VPC to privately connect to Qiskit Runtime](/docs/quantum-computing?topic=quantum-computing-vpe-connection) topic for instructions. Once you create a Virtual Private Endpoint (VPE) gateway, you will be able to use the private endpoints:

```bash
  https://qiskitruntime.private.$REGION.quantum-computing.cloud.ibm.com
  https://scheduler.private.$REGION.quantum-computing.cloud.ibm.com
```
{: codeblock}

where `$REGION` is one [IBM Cloud region](https://cloud.ibm.com/docs/overview?topic=overview-locations#table-mzr){: external}.


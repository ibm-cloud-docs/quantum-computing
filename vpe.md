---

copyright:
  years: 2024
lastupdated: "2024-05-23"

keywords: private endpoints for Qiskit Runtime, VPE for Qiskit Runtime

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}

# Using virtual private endpoints for VPC to privately connect to Qiskit Runtime
{: #vpe-connection}

{{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC lets you connect to Qiskit Runtime from your VPC network by using IP address that you specify, allocated from a subnet within your VPC.
{: shortdesc}

VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per-service basis. The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud}} service on the private backbone. VPE for VPC lets you control all private addressing within your cloud. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

Within Qiskit Runtime, all customer data is transmitted over the private network regardless of whether it is accessed through a public endpoint or VPE. If you ware using the Bring Your Own Bucket feature (`ibmcloud_cos` remote storage type), Qiskit Runtime uses the [{{site.data.keyword.cloud}} Object Storage private endpoints](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints). Customers for whom data locality is a priority should ensure that they access the Cloud Object Storage buckets through the private endpoints. See [Object Storage documentation](/docs/cloud-object-storage?topic=cloud-object-storage-vpes) for further details.

Connecting to Qiskit Runtime through the public endpoints transmits all request and response data over the public internet. To connect to Qiskit Runtime by using a VPE, you must use the Qiskit Runtime API or SDK. The [{{site.data.keyword.cloud}} console Qiskit Runtime page](https://cloud.ibm.com/quantum) can only be accessed through the public network.

## Before you begin
{: #prereq-service-endpoint}

Before you target a VPE for Qiskit Runtime complete the following steps:

* [Create a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).
* [Plan for the network topology](/docs/vpc?topic=vpc-planning-considerations) to connect to VPEs.
* [Set access controls](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways) for your VPE.
* Understand the [limitations](/docs/vpc?topic=vpc-limitations-vpe) of having a VPE.
* Understand how to [view VPE details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway).

## Set up a VPE for Qiskit Runtime
{: #endpoint-setup}

There are several ways to create a VPE gateway. If you use the [CLI](https://cloud.ibm.com/docs/vpc?topic=vpc-vpc-reference&interface=cli#vpe-clis) or [API](https://cloud.ibm.com/apidocs/vpc/latest#create-endpoint-gateway), you must specify the [Cloud Resource Name (CRN)](/docs/account?topic=account-crn) of the region in which you want connect to Qiskit Runtime. Review the following table for the available regions and CRNs.

| Region | Plans   | Fully Qualified Domain Name (FDQN) | Cloud Resource Name (CRN) |
|-----------------|-----------------|-----------------|-----------------|
| `eu-de` | `Standard` | `private.eu-de.quantum-computing.cloud.ibm.com` | `crn:v1:bluemix:public:quantum-computing:eu-de:::endpoint:qiskit-runtime.private.eu-de.quantum-computing.cloud.ibm.com` |
|  | `Channel Partner` | `qiskitruntime.private.eu-de.quantum-computing.cloud.ibm.com`, `scheduler.private.eu-de.quantum-computing.cloud.ibm.com` | `crn:v1:bluemix:public:quantum-computing:eu-de:::endpoint:qiskit-runtime.private.eu-de.quantum-computing.cloud.ibm.com` |
{: caption="Table 1. Region availability, Fully Qualified Domain Names and Cloud Resource Names for connecting Qiskit Runtime over {{site.data.keyword.cloud_notm}} private networks" caption-side="bottom"}

### Configuring an endpoint gateway
{: #endpoint-gateway-servicename}

To configure a VPE gateway, follow these steps:

1. List the available services, including {{site.data.keyword.cloud_notm}} infrastructure services available (by default) for all VPC users.
1. [Create an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for Qiskit Runtime that you want to be privately available to the VPC.
1. [Bind a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) to the endpoint gateway.
1. [View the created VPE gateways](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway) associated with Qiskit Runtime.

Now your virtual server instances in the VPC can access your Qiskit Runtime instance privately.

## Use your VPE for Qiskit Runtime
{: #using-servicename-vpe}

After you create an endpoint gateway for Qiskit Runtime, follow these steps:

### Use the VPE with `qiskit-ibm-runtime` (Python SDK)
{: #vpe-sdk}
{: api}

VPE support requires `qiskit-ibm-runtime` 0.24.0 or later.
{: note}

When instantiating `QiskitRuntimeService`, specify `private_endpoint=True`.

```python
service = QiskitRuntimeService(token="APIKEY", instance="SERVICE_CRN", channel="ibm_cloud", private_endpoint=True)
```
{: codeblock}

### Use the VPE with the Qiskit Runtime API
{: #vpe-api}
{: api}

After creating an endpoint gateway for Qiskit Runtime, use the service endpoint's FQDN for the target region.

```bash
  curl -X POST https://private.$REGION.quantum-computing.cloud.ibm.com/jobs -H "Authorization: Bearer $BEARER_TOKEN" -H "Service-CRN: $SERVICE_INSTANCE_CRN" -d '{
    "backend": "backend",
    "program_id": "sampler"
  }'
```
{: codeblock}

{: pre}

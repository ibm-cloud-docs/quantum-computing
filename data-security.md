---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-20"

keywords: data encryption in Qiskit Runtime, data storage for Qiskit Runtime, personal data in Qiskit Runtime, data deletion for Qiskit Runtime, data in Qiskit Runtime, data security in Qiskit Runtime,

subcollection: quantum-computing

---

{{site.data.keyword.attribute-definition-list}}

_Name your file `data-security.md` and include it in the **How to** nav group in the **Enhancing security** topic group in your `toc.yaml` file. See https://test.cloud.ibm.com/docs/writing?topic=writing-security-content-guidance_

_Make sure that you use the following title for your topic._

# Securing your data in Qiskit Runtime
{: #mng-data}



_The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words. Summarize your offering's support for customer-manageds key encryption and processes for handling and deleting data. See the following example for a service that supports Key Protect for BYOK and Hyper Protect Crypto Services for KYOK._

To ensure that you can securely manage your data when you use Qiskit Runtime, it is important to know exactly what data is stored and encrypted and how you can delete any stored data.
{: shortdesc}



## How your data is stored and encrypted in Qiskit Runtime
{: #data-storage}

_Document how your offering stores and encrypts user data as it relates to user activities. What data is encrypted and what is not? How is data encrypted?_

_If you are using separate keys to encrypt each customer, provide that information here._

_Do not include any details that would give malicious persons an advantage to compromising your offering environment._


## Protecting your sensitive data in Qiskit Runtime
{: #data-encryption}

_Document if your services supports Key Protect and/or Hyper Protect Crypto Services for customer-managed keys (whether it's using BYOK or KYOK or both methods). Explain what data is encrypted with customer-managed keys and what data is not. The following is example text from a Watson service that uses Key Protect for BYOK:_

The data that you store in {{site.data.keyword.cloud_notm}} is encrypted at rest by using a randomly generated key.




## Deleting your data in Qiskit Runtime
{: #data-delete}

_Document how users can delete their data within the service._

Deleting a service instance removes all of the content associated with that instance, such as your jobs, results, parameters, and programs. To delete an instance, from the [Instances page](https://cloud.ibm.com/quantum/instances), find the instance you want to remove, click its overflow menu, then click **Delete**. You will be asked to confirm the deletion. 

### Deleting Qiskit Runtime instances
{: #service-delete}

_Include information about whether deleting the service fully erases all data. If deleting the service doesn't remove all personal data, include information about how users can completely delete their data._

_Information about how long services keep data after instances are deleted is covered in the service description. Include the following reference for users to find their data retention period._

The Qiskit Runtime data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the Qiskit Runtime service description, which you can find in the [{{site.data.keyword.cloud_notm}} Terms](https://www.ibm.com/support/customer/csol/terms?id=i126-9425&lc=en#detail-document){: external}.

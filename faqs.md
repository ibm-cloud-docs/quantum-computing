---

copyright:
  years: 2022, 2023
lastupdated: "2022-04-14"

keywords: qiskit runtime frequently asked questions, quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: faq

---


{{site.data.keyword.attribute-definition-list}}



# FAQs for IBM Cloud Qiskit Runtime
{: #qiskit-runtime-faqs}


To find all FAQs for {{site.data.keyword.cloud}}, see our [FAQ library](/docs/faqs).
{: shortdesc}

The [new IBM Quantum Platform](https://quantum.cloud.ibm.com/){: external} interface has been released in early access mode.  It is recommended that you start using that interface to work with IBM Quantum services. Because it is built on IBM Cloud, migration is straightforward.  See the [migration guide](https://quantum.cloud.ibm.com/docs/migration-guides/classic-iqp-to-cloud-iqp){: external} for details.
{: attention}

## What is {{site.data.keyword.qiskit_runtime_notm}} service?
{: #what-are-runtimes}
{: faq}

{{site.data.keyword.qiskit_runtime_notm}} service is a runtime environment through the IBM Cloud that provides access to the IBM Quantum processors and simulators. They allow users to run quantum programs, which require specialized quantum hardware that is coupled closely with traditional “classical”, computer hardware.

## Why is IBM launching {{site.data.keyword.qiskit_runtime_notm}} service?
{: #faq-why-runtime}
{: faq}

IBM made quantum computers available through the cloud in 2016. In 2022, IBM integrates with {{site.data.keyword.cloud}} accounts to offer {{site.data.keyword.qiskit_runtime_notm}} API access. This access creates a smoother customer experience and the ability to combine {{site.data.keyword.qiskit_runtime_notm}} with other kinds of cloud compute resources for their particular workflow or application.

## What can {{site.data.keyword.qiskit_runtime_notm}} service *not* do?
{: #faq-runtime-not}
{: faq}

{{site.data.keyword.qiskit_runtime_notm}} service provides access to IBM QPUs (quantum processing units). Today’s QPUs are somewhat constrained in the size of problems that they can address due to available scale and quantum volume. Nonetheless, these QPUs can already be used to solve small problems and to explore this new and exciting field.

## What skills are required to use the {{site.data.keyword.qiskit_runtime_notm}} service?
{: #faq-skills}
{: faq}

The {{site.data.keyword.qiskit_runtime_notm}} service is meant to be accessible to anyone comfortable with Python. Use of {{site.data.keyword.qiskit_runtime_notm}} primitives requires expressing a problem as quantum circuits. The Qiskit application modules can facilitate this task for various application domains such as optimization, chemistry, finance, and machine learning. Creation of novel {{site.data.keyword.qiskit_runtime_notm}} programs requires more knowledge of the Qiskit backend interface.

##	What are the benefits of using Qiskit Runtime?
{: #faq-benefits}
{: faq}

{{site.data.keyword.qiskit_runtime_notm}} provides access to industry-leading quantum hardware, closely coupled with IBM Cloud resources to enable optimized computing. Qiskit Runtime enables clients to experiment, learn, and prepare for a quantum-accelerated future.

##	What are {{site.data.keyword.qiskit_runtime_notm}} primitives?
{: #faq-primitive}
{: faq}

The {{site.data.keyword.qiskit_runtime_notm}} primitives define abstract interfaces for common tasks that are found in quantum applications. In particular, the Sampler primitive allows a developer to investigate a nonclassical quasi-probability distribution produced by the output of a quantum circuit. The Estimator primitive allows a developer to measure quantum observables on the output of quantum circuits.

##	How is the {{site.data.keyword.qiskit_runtime_notm}} service offering different from IBM Quantum Experience?
{: #faq-iqx}
{: faq}

Whether accessing it through {{site.data.keyword.cloud}} or directly through IBM Quantum Experience, users can harness the power of Qiskit Runtime. {{site.data.keyword.qiskit_runtime_notm}} on {{site.data.keyword.cloud}} allows users to pay only for what they use, and also makes it easy to integrate your quantum computing work with your other {{site.data.keyword.cloud}} tools.

##	Which URLs should I add to our firewall whitelist for IBM Quantum access?
{: #faq-whitelist}
{: faq}

* **IBM Quantum API**: ``*.quantum-computing.ibm.com``
* **IBM Cloud Quantum API**: ``*.quantum-computing.cloud.ibm.com``
* **IBM Cloud**: ``*.cloud.ibm.com``

##	What plans are available to use {{site.data.keyword.qiskit_runtime_notm}} with {{site.data.keyword.cloud}}?
{: #faq-plans}
{: faq}

Currently, there are two plans. The Lite plan (deprecated) allows the user to access only quantum simulators and is free of charge. Pay-as-you-go access to IBM Quantum hardware and simulators is provided with the Standard plan.  For more information, see [Manage the cost](/docs/quantum-computing?topic=quantum-computing-cost).

##	What is the cost of the {{site.data.keyword.qiskit_runtime_notm}} Standard plan?
{: #faq-pricing}
{: faq}

The {{site.data.keyword.qiskit_runtime_notm}} Standard plan is a pay-as-you-go service and costs $1.6 per second when running on physical QPUs (quantum processing units). For more information, see the [Qiskit Runtime Standard plan](/docs/quantum-computing?topic=quantum-computing-cost) topic.

##	What is the pricing metric of the {{site.data.keyword.qiskit_runtime_notm}} service?
{: #faq-pricing-metric}
{: faq}

For this service, you are charged for job execution time.  Job execution usage is the amount of time that the QPU is dedicated to processing your job. Queue time is not included. For more information, see the [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-plans) topic.


##	Can I use {{site.data.keyword.qiskit_runtime_notm}} with my {{site.data.keyword.cloud}} lite account?
{: #faq-Lite}
{: faq}

Yes, but with the Lite plan (deprecated) you can access only quantum simulators. To use IBM QPUs, you need to upgrade to an IBM pay-as-you-go cloud account and use the Standard plan.

##	Do I get a monthly bill and what does it look like for {{site.data.keyword.qiskit_runtime_notm}} with {{site.data.keyword.cloud}}?
{: #faq-bill}
{: faq}

You will receive a monthly invoice that provides details about your resource charges. You can check how much you've spent at any time on the [IBM Cloud Billing and usage page](https://cloud.ibm.com/billing){: external}.

You can set up spending notifications to get notified when your account or a particular service reaches a specific spending threshold that you set. For information, see the [IBM Cloud account **Type** description](/docs/account?topic=account-accounts){: external}. {{site.data.keyword.cloud}} spending notifications trigger only _after_ cost surpasses the specified limit.

##	Are there countries from which the Qiskit Runtime service is not available?
{: #faq-countries}
{: faq}

{{site.data.keyword.qiskit_runtime_notm}} (beta) is unavailable from the following countries (as of April 2022): Armenia, Azerbaijan, Belarus, Cambodia, China (including Hong Kong S.A.R. of the PRC), Cuba, Georgia, Iraq, Iran, Kazakhstan, Kyrgyzstan, Laos, Libya, Macao S.A.R. of the PRC, Moldova, Mongolia, Myanmar (Burma), North Korea, Russia, Sudan, Syria, Tajikistan, Turkmenistan, Ukraine, Uzbekistan, Venezuela, Vietnam, and Yemen.

## How are jobs prioritized, as the available QPUs are shared among all users in the {{site.data.keyword.qiskit_runtime_notm}} service?
{: #faq-share}
{: faq}

Jobs are prioritized through a first in first out method.

## Can I choose a specific QPU from those offered by Qiskit Runtime?
{: #faq-choose-system}
{: faq}

Yes. {{site.data.keyword.qiskit_runtime_notm}} allows you to specify the QPU on which your Qiskit program should be run.

## Is there support provided for {{site.data.keyword.qiskit_runtime_notm}} and what is the Service Level Objective?
{: #faq-service}
{: faq}

Currently, the {{site.data.keyword.qiskit_runtime_notm}} cloud service is in beta status. Therefore, IBM provides best effort support for the service. IBM uses commercially reasonable efforts to respond to support requests; however, there is no specified response time objective for support.

## How can I open a support ticket if I encounter a problem with the service or my jobs?
{: #faq-help}
{: faq}

For help with Qiskit, access our Slack community: [Qiskit Slack](https://qisk.it/join-slack){: external}.


## {{site.data.keyword.qiskit_runtime_notm}} is in beta status. When will {{site.data.keyword.qiskit_runtime_notm}} be generally available in {{site.data.keyword.cloud}}?
{: #faq-beta}
{: faq}

The {{site.data.keyword.qiskit_runtime_notm}} beta service is constantly enhanced with new features and functions based on feedback from our users. Enhancements from quantum hardware and software might also contribute to more features and functions of the service. The integration of the service in {{site.data.keyword.cloud}} creates many possibilities to interact with other services on {{site.data.keyword.cloud}}. At the moment, no specific plan for a general availability exists.

## Today there are two primitive programs available to access IBM QPUs. Are there plans to provide more primitives? For instance, for more low-level access to IBM QPUs?
{: #faq-more-primitives}
{: faq}

We continue to evaluate the use of quantum hardware and expand the set of primitives to meet common needs. However, the {{site.data.keyword.qiskit_runtime_notm}} in {{site.data.keyword.cloud}} is meant to serve algorithm or application development. If you require lower-level access, then your needs would be better served by the [IBM Quantum channel](https://quantum.ibm.com/){: external}.

## Can a {{site.data.keyword.qiskit_runtime_notm}} service instance be shared between users?
{: #faq-share-instance}
{: faq}

{{site.data.keyword.qiskit_runtime_notm}} supports {{site.data.keyword.cloud}} Identity and Access Management (IAM). With IAM, the user who deployed the service can enable users and groups to access the service. The user who deployed the service gets charged for the usage of all users who are enabled for that service instance.

## How long does it take to deploy {{site.data.keyword.qiskit_runtime_notm}} in {{site.data.keyword.cloud}}?
{: #faq-how-long}
{: faq}

The service can be deployed in about 10 seconds and can be used immediately after it appears in the {{site.data.keyword.cloud}} account resource list.

## How can I access IBM QPUs after the Qiskit Runtime service is deployed in IBM Cloud?
{: #faq-access-systems}
{: faq}

 Make sure to use the Standard plan when you deploy a service instance of Qiskit Runtime, as described in the [Getting started guide](/docs/quantum-computing?topic=quantum-computing-get-started).


## What programing languages can I use to program against the primitives?
{: #faq-languages}
{: faq}

The Cloud service API is programming language independent. However, Qiskit provides a comprehensive framework for quantum computing. Qiskit uses and supports Python.


## Why can't I run my uploaded custom program?
{: #faq-custom-pgm}
{: faq}

Running, adding, or changing custom programs are not supported on IBM Cloud Qiskit Runtime. If you used this function previously, you can instead use code that calls primitives. To get  performance benefits comparable to uploaded programs, you can use use [sessions](https://docs.quantum.ibm.com/guides/sessions){: external}, which are a service aware context manager that minimizes artificial queuing latency inside an iterative workload.

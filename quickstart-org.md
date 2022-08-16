---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-16"

keywords: quantum, Qiskit, runtime, near time compute, university, business, organization

subcollection: quantum-computing

content-type: tutorial
completion-time: 25m
---

{{site.data.keyword.attribute-definition-list}}

# Plan Qiskit Runtime for an organization
{: #quickstart-org}
{: toc-content-type="tutorial"}
{: toc-completion-time="25m"}

When working in an organization where individuals might work on several projects, the governance of consuming Qiskit Runtime can seem complex. However, access management can be used to easily enable collaboration by users who work on the same project, as well as to restrict visibility of users and projects that should be isolated from each other. Managing access becomes particularly relevant when sharing Qiskit Runtime resources that are not free; that is, Qiskit Runtime service instances that use the Standard plan (which organizations are charged for).
{: shortdesc}

## Overview
{: #overview-org}

IBM Cloud provides various ways to implement these mechanisms described in this tutorial.  There might be several ways to achieve these objectives. Additionally, most of the steps in this tutorial are generic to IBM Cloud and not specific to Qiskit Runtime, except the custom role details.
{: note}

### Involved Personas
{: #personas-org}

The are several main personas mentioned in this tutorial:

* **User**: Someone who gets access to Qiskit Runtime resources (_service instances_) and can potentially collaborate with other users on these resources. Users' access is controlled by an administrator and they cannot create or delete service instances.
* **Cloud administrator**: An IBM Cloud account owner who owns Qiskit Runtime resources and manages which users can access these resources. As the resource owner, the administrator is charged for any paid resource use.
* **IDP administrator**: An administrator who defines identities and their attributes in an identity provider (IDP).

### Terminology
{: #terms-org}

This tutorial uses the following terms:

* _Resource_: A generic IBM Cloud term that refers to an object that can be managed through the Cloud user interface, CLI, or API. For this tutorial, a _resource_ is a Qiskit Runtime service instance.
* _Service instance_: A service instance is used to access Cloud functionality. Specifically, quantum computing on real devices or simulators. It is defined through the catalog. You can define several service instances based on the same or different plans, which offer access to different quantum computing backends. See [Qiskit Runtime plans](/docs/quantum-computing?topic=quantum-computing-cost) for more details.
* _Project_: A grouping unit that enables users to work on the same resources. This tutorial uses two projects; `ml` and `finance`. See [Hierarchical project structures](/docs/quantum-computing?topic=quantum-computing-considerations-org#nest-org) for more information.

   This project is not related to the "project" concept in IBM Quantum Platform.
   {: note}

## Plan your setup
{: #planning-org}
{: step}

Before setting up Qiskit Runtime for your organization, you need to decide the following:

* How will user identities be defined? You can set up IBM Cloud users, users from another IDP, or both.
  * If you are using a different IDP, will the Cloud administrator or the IDP administrator assign  users to project resources?
    * If the IDP administrator performs this assignment, you will need a string to be used as a key, such as `project` (which this tutorial uses) for project comparisons.
* What are the projects and which service instances should belong to each? It is important that you plan your project names carefully.
  * Project names should not be substrings of another.  For example, if you use `ml` and `chemlab` for project names, then later you set up a project match for `ml`, it will trigger on both values, accidentally granting more access than expected. Instead, use unique names such as `ml` and `chem-lab`.  Alternatively, use prefix or suffix values to avoid such unintended substring matches.
  * Appropriately using naming conventions, along with prefix or suffix values can help you easily allow access to several projects.  
  * Quantum experiments (jobs) belong to service instances, and users having access to an instance can see its jobs.
  * Service instances can be based on different plans, allowing access to different backends like real devices or simulators. See [Choose a system or simulator](/docs/quantum-computing?topic=quantum-computing-choose-backend) for details.
* Which users should get visibility to which projects?
* Should users be able to delete jobs? Keeping jobs in service instances gives more traceability for billing costs. This information combines well with the audit trail of [Activity Tracker](/docs/quantum-computing?topic=quantum-computing-considerations-org), which  tracks which user submitted the job.
* Will you use access groups that directly reference Qiskit Runtime service instances or organize services into resource groups?
   * **Access groups** are a convenient and common way of controlling user access for IBM Cloud resources.  They are a simple but powerful means to consistently assign user access. We create a access group for each project and map users to access groups. Each access group uses a custom role that allows users to access specific Qiskit Runtime service instances or resource groups.
   * **Resource groups** are used only when you need to maintain a clear separation of service instances.  If additional service instances are created in a resource group, all users having access to the resource group will see them automatically without updating access groups.  If you choose to use resource groups, you will still create access groups, which will then be assigned to resource groups.

   A service instance can only belong to one resource group, and after instances are assigned into resource groups, they cannot be changed. This also means that the resource group assignment can only happen at service instance creation. Therefore, resource groups may not provide enough flexibility if assignments of service instances to resource groups might need to change.
   {: note}

## Next steps
{: #next-steps-org}

* See [Configure Qiskit Runtime for an organization](/docs/quantum-computing?topic=quantum-computing-quickstart-steps-org) for the steps to set up Qiskit Runtime.
* See [Additional considerations](/docs/quantum-computing?topic=quantum-computing-considerations-org) for more information.  

---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-26"

keywords: question about qiskit errors, error when running qiskit or qiskit runtime commands

subcollection: quantum-computing

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}



# Why am I getting an import error when I run a Qiskit command?
{: #tbs-qiskit-version}
{: troubleshoot}

You're having trouble running Qiskit commands, for example, from a notebook.
{: shortdesc}

You run a Qiskit command but get an error message, starting with an import error.  For example: `ImportError: cannot import name 'SymbolicPulse' from 'qiskit.pulse.library'...`.
{: tsSymptoms}

This can happen when Qiskit or Qiskit Runtime is down-level.
{: tsCauses}

Update Qiskit and Qiskit Runtime by running these commands, then try the command again:
{: tsResolve}

```Python
# Installs the latest version of the Qiskit meta-package.
pip install qiskit -U
```
{: codeblock}

```Python
# Installs the latest version of the Qiskit Runtime package.
pip install qiskit-ibm-runtime -U
```
{: codeblock}

---

copyright:
  years: 2015, 2022
lastupdated: "2022-02-08"

subcollection: quantum-computing

keywords: quantum computing CLI, quantum computing command line , quantum computing terminal, quantum computing shell, quantum, Qiskit, runtime, near time compute

---

{{site.data.keyword.attribute-definition-list}}





# Quantum computing CLI
{: #quantum-computing-cli}



The {{site.data.keyword.cloud}} Quantum Computing command-line interface (CLI) allows you to access {{site.data.keyword.quantum_long_notm}} via the Qiskit Runtime architecture. You can use {{site.data.keyword.cloud_notm}} CLI to manage V2 service brokers and templates.
{: shortdesc}




## Prerequisites
{: #quantum-computing-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install the Quantum Computing CLI by running the following command:

```sh
   ibmcloud plugin install <cli-plugin>
```
   {: pre}

   



You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running **`ibmcloud plugin list`**.
{: tip}



{{site.data.keyword.cloud_notm}} CLI requires Java&trade 1.8.0. You can download the CLI from {{site.data.keyword.cloud_notm}} to use on your local system as a complement to the {{site.data.keyword.cloud_notm}} console.
{: note}



## cloud-cli login
{: #quantum-computing-login}

Use this command to log in to {{site.data.keyword.cloud_notm}}.

```sh
cloud-cli login -a API_ENDPOINT [--sso] [-u USERNAME] [-p PASSWORD] [--apikey KEY | @KEY_FILE]
                [--no-iam] [-c (ACCOUNT_ID | ACCOUNT_OWNER_USER_ID) | --no-account]
		[-g (RESOURCE_GROUP_NAME | RESOURCE_GROUP_ID)]
```


### Prerequisites
{: #quantum-computing-login-prereqs}

### Command options
{: #quantum-computing-login-options}



-a API_ENDPOINT
:   The API endpoint, such as `cloud.ibm.com`. Required.

--sso
:   Specify this option to [log in with a federated ID](/docs/iam?topic=iam-federated_id). Using this option prompts you to authenticate with your single sign-on provider and enter a one-time passcode to log in.

-u USERNAME
:   The {{site.data.keyword.cloud_notm}} user name.

-p PASSWORD
:   The user password.

--apikey API_KEY or @API_KEY_FILE_PATH
:   The API key content or the path of an API key file that is indicated by the @ symbol.

--no-iam
:   Force authentication with the login server instead of the public IAM.

-c ACCOUNT_ID
:   The ID of the target account. This option is exclusive with the `--no account` option.

--no-account
:   Forced login without the account. This option isn't recommended, and it is exclusive with the `-c` option.

-g RESOURCE_GROUP
:   The name or ID of the target resource group.


### Examples
{: #quantum-computing-login-examples}

Log in to {{site.data.keyword.cloud_notm}} by entering `tom` for the user ID and `123` for the password.

```sh
cloud-cli login -a cloud.ibm.com -u tom -p 123
```
{: pre}

### Output
{: #quantum-computing-command-output}

The command returns the following output:

```text
Targeted account tom Account (abc123cf7ca78fb1997fcbd8999106af)

API endpoint:      https://cloud.ibm.com
Region:            us-south
User:              tom
Account:           tom's Account (abc123cf7ca78fb1997fcbd8999106af)
Resource group:    No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
CF API endpoint:
Org:
Space:

Tip: If you are managing Cloud Foundry applications and services
- Use **`ibmcloud target --cf`** to target Cloud Foundry org/space interactively, or use **`ibmcloud target --cf-api ENDPOINT -o  ORG -s SPACE`** to target the org/space.
- Use **`ibmcloud cf`** if you want to run the Cloud Foundry CLI with current IBM Cloud CLI context.
```
{: screen}

## command xxx
{: #quantum-computing-anchor_ID}



## Logging in
{: #quantum-computing-login-cmd}

### cloud-cli login
{: #quantum-computing-cli-login}

Use this command to log in to {{site.data.keyword.cloud_notm}}.

```sh
cloud-cli login -a API_ENDPOINT [--sso] [-u USERNAME] [-p PASSWORD] [--apikey KEY | @KEY_FILE]
                [--no-iam] [-c (ACCOUNT_ID | ACCOUNT_OWNER_USER_ID) | --no-account]
		[-g (RESOURCE_GROUP_NAME | RESOURCE_GROUP_ID)]
```

#### Prerequisites
{: #quantum-computing-cli-login-prereqs}

#### Command options
{: #quantum-computing-cli-login-options}



-a API_ENDPOINT
:   The API endpoint, such as `cloud.ibm.com`. Required.

--sso
:   Specify this option to [log in with a federated ID](/docs/iam?topic=iam-federated_id). Using this option prompts you to authenticate with your single sign-on provider and enter a one-time passcode to log in.

-u USERNAME
:   The {{site.data.keyword.cloud_notm}} user name.

-p PASSWORD
:   The user password.

--apikey API_KEY or @API_KEY_FILE_PATH
:   The API key content or the path of an API key file that is indicated by the @ symbol.

--no-iam
:   Force authentication with the login server instead of the public IAM.

-c ACCOUNT_ID
:   The ID of the target account. This option is exclusive with the `--no account` option.

--no-account
:   Forced login without the account. This option isn't recommended, and it is exclusive with the `-c` option.

-g RESOURCE_GROUP
:   The name or ID of the target resource group.


#### Examples
{: #quantum-computing-cli-login-examples}

Log in to {{site.data.keyword.cloud_notm}} by entering `tom` for the user ID and `123` for the password.

```sh
cloud-cli login -a cloud.ibm.com -u tom -p 123
```
{: pre}

#### Output
{: #quantum-computing-cli-command-output}

The command returns the following output.

```text
Targeted account tom Account (abc123cf7ca78fb1997fcbd8999106af)

API endpoint:      https://cloud.ibm.com
Region:            us-south
User:              tom
Account:           tom's Account (abc123cf7ca78fb1997fcbd8999106af)
Resource group:    No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
CF API endpoint:
Org:
Space:

Tip: If you are managing Cloud Foundry applications and services
- Use **`ibmcloud target --cf`** to target Cloud Foundry org/space interactively, or use **`ibmcloud target --cf-api ENDPOINT -o  ORG -s SPACE`** to target the org/space.
- Use **`ibmcloud cf`** if you want to run the Cloud Foundry CLI with current IBM Cloud CLI context.
```
{: screen}

### command xxx
{: #quantum-computing-new-anchor_ID}

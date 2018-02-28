![](../common/images/customer.logo.png)
---
# ORACLE Cloud-Native DevOps workshop #

## Create Java Cloud Service instance using PSM CLI on Oracle Cloud Infrastructure (JCS on OCI) - PSM CLI JCS on OCI Quick Start ##

### Introduction ###

The Oracle Java Cloud Service (JCS) provides a cloud-based application server (Oracle WebLogic Server with automated customer-controlled provisioning, backup, patching, scaling with cloud tooling) designed to support any Java application. You may use the Oracle Java Cloud Service through the Oracle Java Cloud Service console and quickly create and configure Java EE application environment.

The Oracle Cloud Infrastructure (OCI) platform can run both Oracle workloads and cloud native applications, it offer highly available infrastructure ideal for enterprise applications and provide greater control over the network. This all adds up to great price-performance for Weblogic deployment.

In [JCSonOCI](../jcs-on-oci/README.md) lab we provisioned JCS on OCI using web user interface, Platform Services Console. Now we want to speed up provisioning by using PSM CLI tool. PSM (PaaS Service Manager) is a command line interface (CLI) tool for management of various PaaS services in Oracle Public Cloud, wrapper over PaaS REST APIs that invokes these APIs, so you can use it in your cloud automation scripts. It provides commands for tasks such as creating and deleting services, start/stop/scaling, patching, backup, monitoring and much more.

### About this tutorial ###
This tutorial shows how to use PSM CLI while creating Java Cloud Service (JCS) instance on Oracle Cloud Infrastructure (OCI).

Note: The content of the CLI response is based on the resources defined in your environment. Your response may not match the examples shown in this tutorial.

### Scenario ###
You have subscriptions to Oracle Public cloud and you did setup prerequisites for Oracle Platform Services on Oracle Cloud Infrastructure (first step in JCSonOCI lab), and provisioned Oracle Database Cloud (Database as a Service) instances on OCI (second step in JCSonOCI). 
Now, you want to learn how to create different environments for Java applications using PSM CLI. This tutorial shows you how create three kind of environments. Specifically, you will create the following environments (environments are based on QuickStart Templates with some changes):
-	Simple Java Web App – single node WL EE serve (1 OCPU)
-	Multi-Tier Java EE App with High Availability – two node WL EE cluster with two OTD (4 OCPU)
-	Highly Available Java EE App with Caching – two node WL Suite cluster, two OTD, one node Coherence (5 OCPU)

### Prerequisites ###
To complete this tutorial, you need:
-	Provisioned DBCS on OCI
-	Python 3.3 or later. For more information about downloading and using Python, go to python.org. You will also have to use the PIP tool pip3 to install the CLI Python package.
Note: While installing Python, make sure to select the check box Add Python to PATH.
-	Have the appropriate credentials for working with Oracle Java Cloud Service.

### PSM CLI setup ###
Documentation for PSM CLI setup that includes steps for Windows and Linux platform is [here](https://docs.oracle.com/en/cloud/paas/java-cloud/pscli/using-command-line-interface-1.html), I will show here steps for Windows platform.
First, download PSM CLI binaries from any PaaS service console. Click on your username, then help and then Download Center.
![](images/psmcli-download-01.png)
On the download page click the download image on the right and follow the instructions to save psmcli.zip into folder on local disk.
![](images/psmcli-download-02.png)

Install the PaaS CLI as a Python package. Use the PIP tool pip3 to install the CLI Python package. PIP is already installed if you are using Python 2 >=2.7.9 or Python 3 >=3.4 binaries downloaded from [python.org](https://www.python.org/), if you need to upgrade you can follow this steps for [upgrade pip](https://pip.pypa.io/en/stable/installing/#upgrading-pip).
```
>pip3 install -U psmcli.zip
```
![](images/psmcli-install-01.png)

First psm command that we are using here to check if psm is available and version is:
```
>psm –v
```
![](images/psmcli-install-02.png)

Now we have to connect to Oracle Cloud where we will be manage resources. For this, you can run the setup command, ```psm setup```, and when prompted, enter your cloud user name, password, and identity domain. In order to avoid the prompt-by-prompt routine described above we will use profile. You can create different profiles as JSON files, based on region, credentials, identity domain, and so on and quickly configure your CLI client by passing the profile as a payload. This is helpful if you have multiple configuration profiles (for example, one for each account) as you can use the appropriate profile to access any account.
Before you run psm setup command, if your machine requires a proxy server in order to connect to the Internet, set the http_proxy and https_proxy environment variables.
```
set http_proxy=http://myproxy.oracle.com:80
set https_proxy=https://myproxy.oracle.com:80
```
Below is the example of profile used in this workshop with parameter comments so you cannot copy/paste it as is, remove comment or use source code from here, [oraclecloud-setup.json](scripts/oraclecloud-setup.json).
```
{ 
    "username":" john.smith@example.com", //Username for the account.
    "password":" aZerty12", //Password associated with the specified username.
    "identityDomain":"idcs-35w8456d2e6544e6736s14ffa9866e73", //Identity domain associated with the specified user. If you are configuring your CLI for use with Identity Cloud Service (IDCS), you need to enter your IDCS tenant name rather than your identity domain. The IDCS tenant name is usually a lengthy string of characters preceded by a service ID, for example, idcs-6572bfeb183b4becad9e649bfa14a488. Below are steps to find Identity Domain for IDCS enabled accounts.
    "region":"us", //The cloud data center region”us”, “emea”, “aucom”
    "outputFormat":"short" //Output of psm command, “short”, “json”, “html”
}
```
How to find Identity Domain for IDCS enabled accounts:
To find your IDCS tenant name: when you login, on the Cloud Dashboard page click on any service name, Java in this example.
![](images/psm-setup-01.png)
You will find Identity Service Id on the right side.
![](images/psm-setup-02.png)

Having successfully created profile file and entered data you can run psm command to connect to our cloud account.
```
>psm setup -c oraclecloud-setup.json
```
![](images/psm-setup-03.png)

![](images/psm-setup-04.png)

After successful connection, we are ready for new commands. You can try some of commands below before we start provision environments:
- Description of all commands related to jcs service: ```>psm jcs h```
- List of available services in your account: ```>psm jcs services```
- Description of particular “MyJCS” jcs service: ```>psm jcs service -s MyJCS```
- Checks health of the “MyJCS”service: ```>psm jcs check-health -s MyJCS```

Try those commands also with changing output format to json by adding “-of json” on the end of each command, for example:
```>psm jcs check-health -s MyJCS –of json```

Note that default output was in “short” format since we setup this format in our profile file, if you want to use “json” as default format that you can change this parameter in profile file.

Command that we wil use to provision jcs environment is:
```> psm jcs create-service -c create-jcs-payload.json```

Ful description of that command you can check with:
```>psm jcs create-service h```

We will continue with examples for provisioning different jcs environments. At the beginning of each lab you can see diagram of environment that will be provisioned, then payload file with comments (comments you have to remove if you will use copy/paste) and source code at the end. We will start with simpler environment and adding more details in subsequent environments. Attributes used in payload file are most common one; it is not full list of all attributes.











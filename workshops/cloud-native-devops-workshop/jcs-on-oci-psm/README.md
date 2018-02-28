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
Now, you want to learn how to create different environments for Java applications PSM CLI. This tutorial shows you how create three kind of environments. Specifically, you will create the following environments (environments are based on QuickStart Templates with some changes):
-	Simple Java Web App – single node WL EE serve (1 OCPU)
-	Multi-Tier Java EE App with High Availability – two node WL EE cluster with two OTD (4 OCPU)
-	Highly Available Java EE App with Caching – two node WL Suite cluster, two OTD, one node Coherence (5 OCPU)

### Prerequisites ###
To complete this tutorial, you need:
-	Provisioned DBCS on OCI
-	Python 3.3 or later. For more information about downloading and using Python, go to python.org. You will also have to use the PIP tool pip3 to install the CLI Python package.
Note: While installing Python, make sure to select the check box Add Python to PATH.
-	Have the appropriate credentials for working with Oracle Java Cloud Service.












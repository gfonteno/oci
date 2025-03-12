# OCI Network VCN and Subnets

## Introduction

Estimated Time: 10 minutes

### About Virtual Cloud Networks and Subnets

Virtual Cloud Networks (VCNs) provide customizable and private cloud networks in Oracle Cloud Infrastructure (OCI). Just like a traditional data center network, the VCN provides customers with complete control over their cloud networking environment. This includes assigning private IP address spaces, creating subnets and route tables. [Visit our documentation](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/Overview_of_VCNs_and_Subnets.htm) for more information on Virtual Cloud Networks.

A Subnet is a subdivision of a VCN. Each subnet in a VCN consists of a contiguous range of IPv4 addresses and optionally IPv6 addresses that do not overlap with other subnets in the VCN.

Logs contain critical diagnostic information that tells you how your resources are performing and being accessed. Log groups are logical containers for organizing logs. Logs must always be associated with log groups and you must create a log group to enable the desired log. [Visit our documentation](https://docs.oracle.com/en-us/iaas/Content/Logging/home.htm) for more information on Logging or [Visit our documentation](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/vcn-flow-logs.htm#vcn_flow_logs) for more details on VCN Flow Logs.

### Objectives

In this lab, you will:

* Create a Log Group to organize the subnet Flow Logs
* Build a Virtual Cloud Network (VCN) to provide the foundation for the network access
* Create three public subnets in the VCN to accommodate the client/server communication and the VTAP monitoring
* Enable Flow Logging for each subnet

When you complete the exercise, the network topology will look like the following, one VCN and three subnets.

![vcnsubnetoverview-slide](images/vcnsubnetoverview.png)

### Prerequisites

* Basic knowledge of OCI Networking components and networking.

## Task 1: Select a Home Region

Let's begin.

1. To begin the lab exercise, ensure you are logged into the Oracle Cloud console and select the desired **Home Region** region. The region can be accessed on the menu bar located at top right of the screen.

    * Click **"US East (Ashburn)"**

        ![region-home](images/region-home.png)

        **Note**: This lab can be completed in any OCI region you have access to with the required resources. Based on your account, select the desired region to complete the exercise. For the purpose of this lab we will use the **"US East (Ashburn)"** region.

## Task 2: Create a VCN (Virtual Cloud Network)

We will start with a basic VCN deployment. One of the goals of this **livelab** is also to provide an understanding of OCI constructs needed for deploying the compute instances and creating connections to view the VTAP and Flow Logs. For this reason, we will not use the VCN Wizard which deploys all OCI Gateways and creates basic routing rules. Instead, we will manually create each artifact as needed.

Let's begin.

1. On the Oracle Cloud Infrastructure Console Home page, using the Navigation menu (on top left) click **Networking** and click on **Virtual cloud networks**.

    * Click the Navigation Menu (top left corner)
    * Click **"Networking"**
    * Click **"Virtual cloud networks"**

        ![vcn-navigation](images/vcn-navigation.png)

2. Make sure you have the correct Compartment selected and click **Create VCN** in the **Virtual Cloud Networks** table. The VCN will provide the network foundation for all the components related to the compute instances and required network monitoring.

    * Click **"Create VCN"**
    * Name: **"hol-vcn"**
    * IPv4 CIDR: **"10.1.0.0/16"** (Press enter)
    * Click **"Create VCN"**

        ![vcn-createvcn](images/vcn-createvcn.png)

        **Note**: If not directed to update/change a field, leave everything else as default.

3. The **VCN** is created, you can now move forward to the **Next Task**.

    ![vcn-list](images/vcn-list.png)

    **Note**: Upon creating the **VCN** the workflow will take you directly to the subnet creation page.

## Task 3: Create VCN Subnets

Now that we have a VCN created, we will create three subnets for the compute and network resources.

Let's begin.

1. In the Subnets table, Click **Create Subnet** to create the first subnet. The first subnet will provide network access for the compute instance representing the client.

    * Click **"Create Subnet"**
    * Name: **"hol-vcn-snet1"**
    * IP Type: **"IPv4 CIDR Block"**
    * IPv4 CIDR: **"10.1.1.0/24"**
    * Click **"Create Subnet"**

        ![subnet-createsubnet1](images/subnet-createsubnet1.png)

        **Note**: We will repeat this step for the additional subnets.

2. In the Subnets table, Click **Create Subnet** to create the second subnet. The second subnet will provide network access for the compute instance representing the server.

    * Click **"Create Subnet"**
    * Name: **"hol-vcn-snet2"**
    * IP Type: **"IPv4 CIDR Block"**
    * IPv4 CIDR: **"10.1.2.0/24"**
    * Click **"Create Subnet"**

        ![subnet-createsubnet2](images/subnet-createsubnet2.png)

3. In the **Subnets table**, Click **Create Subnet** to create the third subnet. The third subnet will provide network access for the compute instance representing the vtap monitoring host.

    * Click **"Create Subnet"**
    * Name: **"hol-vcn-snet3"**
    * IP Type: **"IPv4 CIDR Block"**
    * IPv4 CIDR: **"10.1.3.0/24"**
    * Click **"Create Subnet"**

        ![subnet-createsubnet3](images/subnet-createsubnet3.png)

4. The **subnets** are created, you can now move forward to the **Next Task**.

    ![subnet-list](images/subnet-list.png)

## Task 4: Update Security List

Unlike other security lists, the default security list comes with an initial set of stateful rules, which should in most cases be changed to only allow inbound traffic from authorized subnets relevant to the region that homes that VCN or subnet. For this exercise we will modify the default security list to include the VTAP port UDP 4789. VTAP uses this port/protocol to send collected flows to the desired monitor.

Let's begin.

1. On the Oracle Cloud Infrastructure Console Home page, using the Navigation menu (on top left) click **Networking** and click on **Virtual cloud networks**.

    * Click the Navigation Menu (top left corner)
    * Click **"Networking"**
    * Click **"Virtual cloud networks"**

      ![vcn-navigation](images/vcn-navigation.png)

2. Select the VCN.

    * Select VCN **"hol-vcn"**

      ![cn-vcn](images/vcn-vcn.png)

3. On the VCN page, under **Resources** click **Security List**.

    * Click **"Security Lists"**

      ![vcn-securitylists](images/vcn-securitylists.png)

4. On the Security list page, click **Default Security List**

    * Click **"Default Security Lists for vcn-lab"**

      ![vcn-selectdefaultsl](images/vcn-selectdefaultsl.png)

5. On the Default Security Lists page under **Ingress Rules** click **Add Ingress Rules**.

    * Click **"Add Ingress Rules"**

      ![vcn-addingressrulessl](images/vcn-addingressrulessl.png)

6. On the Add Ingress Rules page configure the rule.

    * Source CIDR: **"0.0.0.0/0"**
    * Select IP Protocol: **"UDP"**
    * Destination Port Range: **"80"**
    * Click **"Add Ingress Rules"**

      ![vcn-addingressrulesslfinish](images/vcn-addingressrulesslfinish.png)

7. Click **Oracle Cloud** in the top left of the menu bar to return to the home page.

    * Click **"Oracle CLoud"**

        ![oraclecloud-home](images/oraclecloud-home.png)

**Congratulations!** You have successfully created a VCN and the associated Subnets. You may now **proceed to the next lab**.

## Acknowledgements

* **Author** - Gabriel Fontenot, Principal Cloud Architect, OCI Networking
* **Last Updated By/Date** - Gabriel Fontenot, March 2025
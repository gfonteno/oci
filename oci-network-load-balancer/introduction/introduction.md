# Introduction

## About this Workshop

Estimated Time: 30 - 45 minutes

The Oracle Cloud Infrastructure Flexible Network Load Balancer service, also called Network Load Balancer, provides traffic distribution from one entry point to multiple backend servers in your virtual cloud network (VCN). The Network Load Balancer, NLB, operates at the connection level and load balances incoming client connections to healthy backend servers based on Layer 3 and layer 4 network flows. Visit our [Network Load Balancer documentation](https://docs.oracle.com/en-us/iaas/Content/NetworkLoadBalancer/overview.htm). In this workshop we will explore a common use-case and look into step-by-step guides on implementing a network load balanced application.

To gain more familiarity with Oracle Cloud's networking concepts, I would recommend watching our introductory [Oracle Cloud Networking YouTube series](https://youtu.be/mIYSgeX5FkM) as well as the [Oracle Cloud Networking Overview Documentation](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/overview.htm).

### Objectives

In this workshop, you will learn how to:

* Deploy a Virtual Cloud Network (VCN), Subnets, and associated Security Lists
* Deploy an Internet Gateway and update the routing table accordingly
* Deploy a NLB and confirm the application distributes the load accordingly

### Prerequisites

This lab assumes you have:

* Administrative Oracle Cloud Access to provision networking and compute resources (free tier account will suffice).
* Basic knowledge of OCI Networking components and networking.

### Workflow

This workshop contains multiple labs with associated tasks. Each lab depends on constructs deployed in previous. As such **do not skip** any lab or task as skipping labs or tasks may result in missing the overall objective of the exercise.

For deploying resources of any kind, OCI asks for an IAM Compartment. Every resource deployed in this workshop will be deployed in the same **LAB** Compartment. Make sure you are the admin of your environment or have sufficient privileges to deploy resources in the Compartment you plan to use.

## Acknowledgements

* **Author** - Gabriel Fontenot, Principal Architect, OCI Networking
* **Last Updated By/Date** - Gabriel Fontenot, March 2025
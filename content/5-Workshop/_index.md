---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## Main workshop: Policy Assistant

Start with **5.7 Policy Assistant** below: local/deployment/testing/monitoring/cleanup, student architecture and COST. Source is locally checked; AWS acceptance is pending. Chapters 5.1–5.6 are a separate S3/VPC learning lab, not the application architecture.



> This is an independent S3/VPC Endpoint workshop, not the Policy Assistant deployment guide or proof that the application uses VPC Endpoints.


# Secure Hybrid Access to S3 using VPC Endpoints

#### Overview

**AWS PrivateLink** provides private connectivity to AWS services from VPCs and your on-premises networks, without exposing your traffic to the Public Internet.

In this lab, you will learn how to create, configure, and test VPC endpoints that enable your workloads to reach AWS services without traversing the Public Internet.

You will create two types of endpoints to access Amazon S3: a Gateway VPC endpoint, and an Interface VPC endpoint. These two types of VPC endpoints offer different benefits depending on if you are accessing Amazon S3 from the cloud or your on-premises location
+ **Gateway** - Create a gateway endpoint to send traffic to Amazon S3 or DynamoDB using private IP addresses.You route traffic from your VPC to the gateway endpoint using route tables.
+ **Interface** - Create an interface endpoint to send traffic to endpoint services that use a Network Load Balancer to distribute traffic. Traffic destined for the endpoint service is resolved using DNS.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-prerequiste/)
3. [Access S3 from VPC](5.3-s3-vpc/)
4. [Access S3 from On-premises](5.4-s3-onprem/)
5. [VPC Endpoint Policies (Bonus)](5.5-policy/)
6. [Clean up](5.6-cleanup/)
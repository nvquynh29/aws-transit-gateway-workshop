---
title : "Cross Account - Single Region"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
In this section, we will configure AWS Transit Gateway to connect VPCs located in different accounts but same region.

**Scenario**: As your company attracts more prominent clients, system security becomes increasingly crucial. 
However, your company does not have any security experts. Therefore, the board of directors decides to collaborate with 
a specialized security firm to conduct penetration testing to identify potential security vulnerabilities. 
Your partner company, with extensive experience in the security domain, understands the importance of early vulnerability 
detection and requests to conduct testing on the Dev environment of your system. They have already set up the necessary 
servers and tools on a VPC named Partner within the same region as your company's infrastructure.

Your job is to connect the Partner VPC with Dev VPC so that the partner company's security experts can test in early stages
of development. Because your company may cooperate with many partners in the future, for easily manage, configure and 
troubleshoot the network, you must use Transit Gateway for configuration instead of VPC Peering. You can absolutely use 
the peering configuration of two transit gateways as in the previous section. However, the partner company has cooperated 
with many customers so their network configuration has become very complex, they will be happy if they do not need to manage 
additional network configuration when connecting to your company's network. In this case, you can use an AWS service called 
**AWS Resource Access Manager (AWS RAM)** to simplifies the network configuration for the partner company. Please note that 
you can only use this service to connect VPCs located in the same region. Your network configuration will look like this:
![Diagram](/images/5-cross-account-single-region/cross_account_single_region.svg)

#### Content

1. [Preparation](5.1-preparation/)
2. [Configure routing](5.2-configure-route-tables/)
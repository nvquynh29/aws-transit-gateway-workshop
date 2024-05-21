---
title : "AWS Transit Gateway"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---

In this section we will learn what AWS Transit Gateway is and why we need this feature.

#### Amazon VPC and VPC Peering
Amazon Virtual Private Cloud (VPC) is an isolated virtual network within the AWS Cloud. Amazon VPC is very similar to 
traditional networks, allows users to deploy servers, databases, and other computing resources.

In fact, there are many cases where we need to connect two or more VPCs to share resources and data between these networks.
The simplest way to connect these networks is through the Internet connection, however this method has many potential security risks,
is unreliable and does not guarantee performance.

**VPC Peering** is a feature that allows connecting two or more Amazon VPCs together through AWS's private network,
**do not** use Internet connection. When using VPC Peering to connect 2 VPCs, the resources in one network can
connect to resources in the remaining network via private IP addresses as if they were in the same network.
![VPC Peering](/images/1-introduction/vpc_peering.svg)

While VPC Peering is an effective feature for connecting VPCs together, it also has some drawbacks. 
One of these is that it can be cumbersome to manage a large number of connections when connecting multiple VPCs. 
VPC Peering does not have a transitive property, meaning that if VPC A is peered with VPC B and VPC B is peered with VPC C, 
VPC A cannot connect to VPC C. Additionally, an Amazon VPC cannot be peered with too many other VPCs.
![VPC Peering Overwhelmed](/images/1-introduction/vpc_peering_overwhelmed.svg)
When you need to connect multiple VPCs together, the routing configuration becomes very complex, difficult to manage and troubleshoot.

#### AWS Transit Gateway
**AWS Transit Gateway** is a powerful VPC feature that enables connecting multiple VPCs, VPN connections, and on-premises networks to a single gateway. It simplifies network configuration by eliminating peer connections between VPCs, instead connecting them to a central hub that acts as a network traffic router. Compared to VPC Peering, AWS Transit Gateway offers several advantages:
- Enhanced Performance: Transit Gateway delivers superior performance by leveraging a centralized routing mechanism, reducing the overhead associated with individual VPC peering connections.
- Simplified Management: It streamlines network management by eliminating the need to manage complex peering relationships between VPCs. Instead, all connections are managed through a single hub, providing a more unified and manageable network topology.
- Scalability: Transit Gateway is highly scalable, capable of handling a large number of VPCs and connections without compromising performance or manageability. This makes it ideal for large-scale, dynamic cloud environments.
- Improved Troubleshooting: Troubleshooting network issues becomes easier with Transit Gateway, as the centralized architecture provides a clear overview of network traffic and simplifies the identification of routing problems.

Due to these advantages, AWS Transit Gateway has become a popular choice for large-scale cloud deployments with complex network requirements. It is particularly well-suited for organizations that need to interconnect multiple VPCs, VPN connections, and on-premises networks while maintaining high performance, manageability, and scalability.
![AWS Transit Gateway](/images/1-introduction/aws_transit_gateway.svg)

To effectively utilize Transit Gateway, it's crucial to understand the different scenarios involved:
- VPCs in the same AWS Account and region
- VPCs in the same AWS Account, different regions
- VPCs in different AWS Accounts, same region
- VPCs in different AWS Accounts, different regions

By understanding these scenarios, you can effectively utilize AWS Transit Gateway to connect multiple VPCs across 
various configurations, simplifying network management and enhancing network performance for complex cloud deployments.
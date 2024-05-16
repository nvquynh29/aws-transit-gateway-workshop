---
title : "AWS Transit Gateway Routing"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

In this section, we will learn about the routing mechanism of AWS Transit Gateway.

#### AWS Transit Gateway Attachment
**AWS Transit Gateway Attachment** serves as a logical connection between a resource and AWS Transit Gateway. This resource 
can be a VPC, VPN connection, Direct Connect gateway, or other supported resource types. When a new Transit Gateway 
attachment is created, it gets associated with the default route table of that Transit Gateway. 
However, this behavior can be modified within the Transit Gateway configuration.

Each AWS Transit Gateway attachment can only be associated with one route table. To associate the attachment with a 
new route table, the existing association must be deleted first. When working with AWS Transit Gateway, if you encounter 
an error while associating an attachment with a route table, check if the attachment is already associated with another route table.

Types of AWS Transit Gateway Attachments:
- **VPC**: Connects a VPC to Transit Gateway
- **VPN**: Connects a VPC connection to Transit Gateway
- **Peering Connection**: Connects two Transit Gateways together
- **Connect**: Connects an SD-WAN appliance or on-premises network to Transit Gateway

This workshop will focus on two specific attachment types: **VPC** and **Peering Connection**.

Resources that can connect to Transit Gateway via attachment are shown in the figure below.
![AWS Transit Gateway Attachment](/images/1-introduction/aws_transit_gateway.svg)

#### Route table
**AWS Transit Gateway route table** is a logical entity that holds a collection of routes (route) defining how to forward 
traffic between resources connected to Transit Gateway. When you create a new AWS Transit Gateway, a default route table 
is automatically generated. However, if you disable route table association and route propagation, 
AWS will not create this default route table. You can also create additional route tables for your AWS Transit Gateway.

Each attachment can only be associated with one route table and propagate routes to one or more route tables.

This workshop will focus on two types of route tables: **Transit Gateway route table** and **VPC route table**.
- **Transit Gateway route table**: Manages traffic routing between VPCs, on-premises networks, and attachments 
connected to Transit Gateway.
- **VPC route table**: Manages traffic routing within the scope of a single VPC.

#### Route table association
Transit Gateway route table association establishes a mechanism for automatically propagating routes from a route table 
to associated attachments, ensuring that traffic finds its intended destination without manual intervention.

Each attachment can only be associated with a single route table. A single route table can be associated with multiple attachments.

When an attachment is associated with a routing table, the routes in that routing table are automatically propagated
to this attachment. This means that the routes in the routing table will be used to direct incoming traffic
and go to the resources connected to that attachment.

**Example**: Connect a VPC and the On-Premises Network

Consider *VPC A* connected to Transit Gateway via the *VPC A* attachment. Creating a custom Transit Gateway route table and 
associating it with the *VPC A* attachment allows you to define a route to an on-premises network (e.g., 10.2.0.0/16). 
This route is automatically propagated to the attachment, enabling resources in *VPC A* to connect to the on-premises network 
without explicit VPC-to-on-premises network routing configuration.
![Route table association](/images/1-introduction/route_table_association.svg)

While route propagation simplifies network management, AWS Transit Gateway Policy Table offers an advanced mechanism for 
filtering and controlling routes propagated to specific attachments. However, due to its complexity, this feature will not 
be covered in this workshop.

#### Route propagation
Within the realm of AWS Transit Gateway, route propagation serves as a fundamental mechanism for automatically distributing 
routes from a route table to associated attachments. This process streamlines network management by eliminating the need 
for manual route configuration for every attachment.

Each attachment (e.g. VPC, VPN, Direct Connect gateway) comes with routes that can be installed in one or more transit gateway 
route tables. When an attachment is propagated to a transit gateway route table, these routes are installed in the route table.

**Example**: Connect two VPCs

Consider VPC A and VPC B connected to Transit Gateway through their respective attachments. 
Creating a custom Transit Gateway route table and associating it with the VPC A attachment. Then you create propagation between
VPC B attachment and this route table. The following route will automatically be added to the route table.

| Destination  | Target  |  Route type |
|---|---|---|
| VPC B CIDR | Attachment ID for VPC B | Propagated |

The newly added route will be propagated to VPC A attachment because it is already associated with this route table,
this configuration allows VPC A to connect to VPC B through Transit Gateway.
![Route propagation](/images/1-introduction/route_propagation.svg)

#### Route table association vs. Route propagation
By understanding the concepts of route propagation and its implementation within AWS Transit Gateway attachments, you can effectively manage network traffic flow, simplify network configuration, and enhance the overall efficiency of your cloud infrastructure.

The key difference is that association determines which route table is used for a specific attachment, while propagation 
ensures the routes in that table are applied to the attachment, enabling connectivity between the connected resources.

Imagine a Transit Gateway as a vast library, each bookshelf representing a route table and each book representing a route. 
Route table association can be likened to granting an individual a library card, providing them with access to the books (routes) 
within the library (route table). Route propagation, on the other hand, is analogous to allowing the individual to borrow books, 
enabling them to utilize the resources (routes) for their intended purpose.

**Example**: E-commerce system with microservices architecture

Consider an e-commerce system employing a microservices architecture, comprising four services: Product, Order, Analyzer, and Visualizer. Each service plays a distinct role:
- Product: Handles product-related requests
- Order: Manages purchase-related requests
- Analyzer: Analyzes product quantity, order volume, revenue, and other metrics
- Visualizer: Utilizes data from Analyzer to generate charts and graphs

Each service is deployed in its corresponding VPC: Product VPC, Order VPC, Analyzer VPC, and Visualizer VPC. All VPCs are 
connected to the same AWS Transit Gateway using the respective attachments: Product Attachment, Order Attachment, Analyzer Attachment, 
and Visualizer Attachment. To enable communication between these VPCs, route configuration is essential.

Due to the urgency of releasing the product to users, the Product and Order services are prioritized for initial release. The development of Analyzer and Visualizer follows. Since Analyzer is still under development and not yet ready for production, its connection to the released services is not immediate. Therefore, we will have two environments: *production* including two services Product, Order and *non-production* including two services Analyzer and Visualizer.

We need to configure the network as follows:
- Product and Order services can be connected to each other
- Analyzer and Visualizer services can connect to each other
- Analyzer and Visualizer services cannot connect to Product and Order services

Utilizing a single route table associated with all attachments is not recommended due to potential issues:
- Security Isolation: Linking all attachments to a single route table compromises security isolation between *production* and *non-production* environments. Different configuration requirements may exist between these environments, limiting flexibility.
- Unnecessary Propagation: Propagating routes from *production* VPCs to *non-production* VPCs is unnecessary as these VPCs do not require direct communication. This configuration adds complexity and potentially slows down routing between networks.

To address these challenges, we can create two custom route tables:
- *ProductionRTB*: Associated with Product Attachment and Order Attachment
- *NonProductionRTB*: Associated with Analyzer Attachment and Visualizer Attachment

This approach effectively defines route tables for specific attachments and segregates configurations between *production* 
and *non-production* environments. The technique of using multiple route tables is widely employed in subsequent sections.

Once Analyzer is ready, it needs to connect to Product and Order for data analysis and revenue reporting. Visualizer only 
requires data from Analyzer, hence connecting Analyzer VPC to Product VPC and Order VPC suffices. This can be achieved by creating propagation between ProductionRTB and Analyzer Attachment. Consequently, a new route enabling traffic routing to Analyzer Attachment 
is automatically generated and propagated to Product and Order attachments.
![Association vs Propagation](/images/1-introduction/association_vs_propagation.svg)

#### Routes for peering attachments
Similar to VPC peering, AWS Transit Gateway enables the interconnection of two Transit Gateways, facilitating seamless 
communication between networks. To achieve this, a peering attachment is created within one Transit Gateway, specifying 
the peer Transit Gateway to establish the connection. Since peering attachments do not automatically propagate routes,
manual configuration of static routes is required to direct traffic to the peered Transit Gateway. 
Once traffic reaches the peered Transit Gateway, it can be further routed to its connected attachments.
![Transit Gateway peering](/images/1-introduction/transit_gateway_peering.svg)

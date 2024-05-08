---
title : "AWS Transit Gateway Routing"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

A Transit Gateway attachment is a logical connection between a resource and an AWS Transit Gateway. The resource can be a VPC, VPN connection, Direct Connect gateway, or other supported resource type.

A Transit Gateway Route Table is a logical entity that contains a set of routes defining how traffic is forwarded between VPCs and on-premises networks connected to the Transit Gateway.

VPC route table: Manages routing within a VPC, controlling how traffic flows between subnets and the internet.
Transit Gateway route table: Manages routing between VPCs, on-premises networks, and other attachments connected to the Transit Gateway.

The key difference is that association determines which route table is used for a specific attachment, while propagation ensures the routes in that table are applied to the attachment, enabling connectivity between the connected resources.

Imagine a library (transit gateway) with different sections (route tables). Association is like giving someone a library card (permission to access sections). Propagation is like allowing them to borrow books (use the routes) from a specific section.
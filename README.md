# VPC Peering Solutions
Peering multiple VPC's with idential CIDR blocks, configuring master route table for proper distrubition and ensuring availability is at an all time high for on-premise DX or other VPC's. 

Our goal was to integrate multiple VPC's with our master VPC without having to re-build or complicate with additional CIDR blocks, the issue was we had overlapping cidr blocks but communication had to be private and with low latency. Peering is not supported when you have overlap, so we must find a solution for this problem. Our core use case was making a handful of EC2 instances talk to one another, and with private fixed addresses I developed the following solution: I proposed that we interconnect our resources using a semi-practical hub and spoke model with our primary. Accepting all traffic via largest suffix principals in order for specific traffic that needed to reach VPC 2 or 3 via static, private routes without additional hops and traffic stays maintained within the region without conversal over our public subnets. I implemented our local route and then initiated our VPC peering connection to the 2nd and 3rd VPC, but due to same prefix and same IP's. So I reworked the destination route to the static IP on the ENI of the instance in VPC 3 and left VPC 2 for primary connection so we have a one-to-one connection back to primary without complicating network logic. Furthermore, this solved the issue of overlapping CIDR without the need to use elastic IP's or reformat our VPC CIDR blocks. 

**Project Architecture:**


![VPC Peering With Conflicting CIDR](https://github.com/NodinVFX/VPCPeering/assets/34975951/916c159c-4009-463d-8b2b-67e7b1d42af7)

This was a great experience labbing, and then implementing on live scale. We had to make a couple adjustments as in our actual infastructure we had an on-premise DX in VPC 2, which needed to be re-worked for the VGW. Due to the VGW not being a peered connection entirely, it was unnecessary to use a static route. We used the VGW route and VPC peered to the 3rd VPC without any complications to the network logic and didn't need to overutilize IP space for this issue.

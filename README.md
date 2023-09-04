# VPC Peering Solutions
Peering multiple VPC's with idential CIDR blocks, configuring master route table for proper distrubition and ensuring availability is at an all time high for on-premise DX or other VPC's. 

I've attached the code used for this project as it was a initial POC used for my job. Our goal was to integrate multiple VPC's with our master without having to re-build the CIDR or complicate with additional CIDR blocks, the issue was we had overlapping cidr blocks but communication had to be private and with low latency. The solution I proposed was interconnecting our resources using a semi-practical hub and spoke model with our primary. Accepting all traffic via largest suffix principals in order for specific traffic that needed to reach VPC 2 or 3 via private request was doable without additional hops and maintained within the region. 


![VPC Peering With Conflicting CIDR](https://github.com/NodinVFX/VPCPeering/assets/34975951/916c159c-4009-463d-8b2b-67e7b1d42af7)

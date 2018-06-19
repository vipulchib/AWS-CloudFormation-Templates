# AWS-CloudFormation-Templates
Miscellenous AWS CloudFormation Templates

## Transit and Edge VPC's with Arista vEOS Routers

AWS CloudFormation templates that we will use will demonstrate the following topology:
![Transit and Edge VPC's with Arista vEOS Routers](https://github.com/vipulchib/AWS-CloudFormation-Templates/blob/master/AWS-Transit_and_Edge-VPC_with_Arista.jpg)

1.  First we will instantiate the Transit VPC based on the topology shown above.  Create a stack using the following command, 
which assumes that AWS CLI is installed:
     ```
     aws cloudformation createstack stackname AristaTransitVPCStack templatebody file://Transit-VPC-us-east-1.yaml
     ```

2.  We will then instantiate the Edge VPC based on the topology shown above.  Ammend the following in the Edge VPC 
CloudFormation template to ensure that you are referencing RouteTableID's for the existing Route Tables in the Transit VPC 
that was created earlier and also reference the Transit VPC's VPC ID:
     - 'RouteTableId' in the 'peerlinkRouteTransitA1/B1/A2/B2' Resources.
     - 'PeerVpcId' in the 'vpcPeer' Resource.

Then create the stack using the following command, which assumes that AWS CLI is installed:
     ```
     aws cloudformation createstack stackname AristaEdgeVPCStack templatebody file://Edge1-VPC-us-east-1.yaml
     ```
3.  I have provided neccessary configs to build the entire routing topology with GRE interfaces:
     ```
     hostname Arista-Transit-1a
     !
	interface Ethernet1
   		mtu 9001
   		no switchport
   		ip address 10.100.1.6/24
	!
	interface Ethernet2
   		mtu 9001
   		no switchport
   		ip address 10.100.11.6/24
	!
	interface Tunnel1
   		description <to-vRouter-1a>
   		mtu 8973
   		ip address 10.0.0.0/31
   		tunnel source 10.100.1.6
   		tunnel destination 10.1.1.6
   		tunnel key 101
	!
	interface Tunnel2
   		description <to-vRouter-1b>
   		mtu 8973
   		ip address 10.0.0.2/31
   		tunnel source 10.100.1.6
   		tunnel destination 10.1.2.6
   		tunnel key 102
	!
	ip route 0.0.0.0/0 Ethernet2 10.100.11.1
	ip route 10.1.1.0/24 Ethernet1 10.100.1.1
	ip route 10.1.2.0/24 Ethernet1 10.100.1.1
	!
	ip routing
	!
	router bgp 65100
   		neighbor edge-routers peer-group
   		neighbor edge-routers fall-over bfd
   		neighbor edge-routers maximum-routes 12000
   		neighbor 10.0.0.1 peer-group edge-routers
   		neighbor 10.0.0.1 remote-as 65101
   		neighbor 10.0.0.3 peer-group edge-routers
   		neighbor 10.0.0.3 remote-as 65101
	!
	end
     ```

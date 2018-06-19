# AWS-CloudFormation-Templates
Miscellenous AWS CloudFormation Templates

## Transit and Edge VPC's with Arista vEOS Routers

AWS CloudFormation templates that we will use will demonstrate the following topology:
![Transit and Edge VPC's with Arista vEOS Routers](https://github.com/vipulchib/AWS-CloudFormation-Templates/blob/master/AWS-Transit_and_Edge-VPC_with_Arista.jpg)

Instantiates a Transit VPC with 2 x Arista vEOS Routers and 2 x Amazon Linux instances.  Create a stack using the following command, which assumes that AWS CLI is installed:
     ```
     aws cloudformation createstack stackname AristaTransitVPCStack templatebody file://Transit-VPC-us-east-1.yaml
     ```

## Edge1-VPC-us-east-1.yaml
Instantiates an Edge VPC with 2 x Arista vEOS Routers and 2 x Amazon Linux instances.  Before executing a stack with this file update the following:
1.  'RouteTableId' in the 'peerlinkRouteTransitA1/B1/A2/B2' Resources.
2.  'PeerVpcId' in the 'vpcPeer' Resource.

Then create the stack using the following command, which assumes that AWS CLI is installed:
     ```
     aws cloudformation createstack stackname AristaEdgeVPCStack templatebody file://Edge1-VPC-us-east-1.yaml
     ```

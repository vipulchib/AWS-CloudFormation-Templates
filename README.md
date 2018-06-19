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

2.  Ee will then instantiate the Edge VPC based on the topology shown above.  Ammend the following in the Edge VPC 
CloudFormation template to ensure that you are referencing RouteTableID's for the existing Route Tables in the Transit VPC 
that was created earlier and also reference the Transit VPC's VPC ID:
     - 'RouteTableId' in the 'peerlinkRouteTransitA1/B1/A2/B2' Resources.
     - 'PeerVpcId' in the 'vpcPeer' Resource.

Then create the stack using the following command, which assumes that AWS CLI is installed:
     ```
     aws cloudformation createstack stackname AristaEdgeVPCStack templatebody file://Edge1-VPC-us-east-1.yaml
     ```

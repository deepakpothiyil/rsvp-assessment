# RSVP Assessment

This repository consists of 3 cloudformation templates used to launch a simple LAMP stack.

Steps:

1. Clone the repository

2. Use the template 'rsvp-vpc' to launch a CloudFormation stack which creates a VPC in your account with 2 public and 2 private subnets.

            Enter a value of your choice to the 'ClassB' parameter to be used for the VPC creation. (For eg, ClassB = 32 would allocate a IPv4 CIDR of 10.32.0.0/16 to the VPC, and corresponding subnet IPV4 ranges for the subnets)

3. Use the template 'rsvp-nat-gateway' to launch another CloudFormation stack to setup a NAT gateway between the public and private subnets created in the previous step. Parameter values to be passed at the time of stack creation are :
        
        ParentVPCStack : The  CF stack name used for step 2 vpc-subnet creation (for eg: rsvp-vpc)
        ParentAlertStack : can be left blank
        SubnetZone : Choose A or B
        
4. Use the template 'rsvp-lamp' to launch a CF stack which will create the following resources:

         - Creates a launch config using a LAMP stack and Linux AMI
         - Application Load Balancer
         - Autoscaling group for self-healing and scaling - starting with 2 instances and can scale upto 5
         - EC2 instances launched by the ASG using the Launch configs, backed by the load balancer.
         - A MySQL RDS instance
         - Security groups for EC2, load balancer and RDS with the necessary port configurations
         - Output URL to access the application
         
    Parameter values to be entered at the launch time are:
    
           - DBAllocatedStorage ('5GB' default for RDS)
           - DBInstanceClass ('db.t2.small' default for RDS)
           - DBName ('rsvpdb' default for RDS)
           - DBPassword (Password for MySQL database (8 to 20 characters - must contain only alphanumeric characters)
           - InstanceType ('t2.small' by default for EC2)
           - KeyName (Select a key from your AWS account which will be used for SSH into EC2)
           - MultiAZDatabase ( 'true' for a HA setup, else 'false' )
           - PublicSubnets ( Select the public subnets created in step 1 )
           - SSHLocation (enter the static ip or VPN ip used in RSVP office)
           - VpcId (VPC id of the VPC created in step 1)
           - WebServerCapacity (2 EC2 instances to start with by default)
  
         
         
         

# AWS Basics

## VPC

By default aws creates a VPC(Virtual Private Cloud) for you. A VPC is like your own little world whcih contains everything you need like IGW, RT, EC2s and all other resources. A VPC can spanned multiple AZs.

The basic diagram looks like this:
![graph](/vpc.png)

### IGW

Internet Gate Way is like a modem that connects to the public internet.

### Route Table

Route Tables are like the Router of your home internet. The Route Table needs to associate with IGW to be able to acceess the internet.

### NACLs

Network Access Control List is like a firewall in a VPC.
You create rules to allow/deny inbound and outbound traffic.

### Subnets

Subnets are like your home network being part of the larger network. A subnet is located in only one AZ.

"Public Subnet" has access to the internet. With IGW attached.

"Private Subnet" does not have access to the internet. Without the IGW attached.

You can create all the subnets and give then names.

You can create different Route Tables with some with IGW connected and some without IGW. Then jsut associate the Subnets to the Route Tables which you want to be publich or privateaccordingly.

## S3

You can choose the S3 class and lifecycle policy (retention policy) tailered to your needs.

S3 Permission can be on the bucket level or object level.

You can make an object public by changing the grantee to everyone and then under "Actions" click on "Make public".

Versioning: once you turn on the Versioning feature you cannot undo it (you can suspend it though). Obviously this is going to increase cost but can be very helpful.

## EC2

EC2 is basically like a desktop computer you have.

### AMI

Amazon Machine Image - A pre-configured package to launch EC2 instance including OS, software etc.

### EBS

Elastic Block Store is basically like a hard drive for an EC2.

You can choose SSD.

For the root EBS, it's like the built-in hard drive to a computer. You can still opt to keep it alive even if the EC2 is deleted
We can technically create EBS volumes and they can be attached to different EC2 instances.

You can create EBS snapshot which is an image of an EBS that can be stored as a buckup of the volume or used to create a duplicate.

### IOPS

Input Output Operation Per Second - the amount of data that can be written to or refefived from EBS per second. This goes hand in hand with EBS, the more EBS the more power for IOPS.

### Security Group

Security Group is like the NACLs but just on the instance level(e.g., EC2, RDS etc) rather than on subnet level. The way allow/deny rules work differently than NACLs.

#### Inbound rules

By default, for a security group everything is denied. So we have to explicitly allow something.

#### Outbound rules,

By default, everything is allowed for outbound.

![securitygroup](/sg.png)

### IP

By default all EC2 instances have an private IP address. Private IP addresses can communicate with each other in the same VPC.

EC2 instances can be lanuched wiht or without a public IP address depending on the subnet settings.

Public IP addresses are REQUIRED for the instances to communicate with the internet.

This is everything for an EC2 to communicate with the internet - which you need to be on a lookout if things don't go as expected
![ip](/ip.png)

#### Example

Create an EC2 Linux instance
--> Select Volumne (SSD)
--> Let's Create a new Security Group
--> Allow Inbound rule SSH protocal 22
--> Allow Inbound rule HTTP protocal 80
--> Create Key/Pair (downalod the key/pair pem file)
To connect to the instance you go to the console and click on connect, it will give you the instructions/commands to ssh into your instance.
You will need to use the {keypair}.pem that you created when provisioning the instance.

## Databse(RDS or Dynamo)

### Dynamo

Dynamo is NoSQL service for AWS.

### RDS

Relational Database Service (SQL).

Just go through the steps - should be strightforward.

Default port is 3306 for RDS MySQL, the point is for every security group you assined you need to have that port allowed.

## SNS

SNS (Simple Notification Service) allows you to automate the sending of email or text message notification based on events that happen in your AWS account.

For example, something happened with the EC2 and it crashed, you want to be notified by email - this is where the SNS can come into play(in conjunction with cloudWatch).

Steps:

- Create a topic
- Add subscriptions (endpoints)
     - e.g., email portocal
       You can then manually publish(trigger) the notification from the console.

## Cloudwatch

CloudWatch is a service that allows you to monitor various elements of your AWS account.

For example, you can use CloudWatch to monitor your EC2 CPU usage, and trigger an alert when it's over 80%.

Or you can monitor your S3 bucket and say if the number of objects in your bucket exceeds 100 then trigger an alert.

You can create a databord and choose the metrics available for your resources.

Then you can create an alarm in the cloudwatch, setting threshold and rules and give it a name for the alarm. The alarm can trigger the SNS topics to send out notifications.

## ELB

Elastic Load Blancer evenly distribute incoming traffice to balance the load.

- Classic load balancer: On TCP layer
- Applicaition load balancer: On http/https layer

## Auto Scaling

Add more or reduce servers based on demand automatically.

Example to set up auto scaling (from the beginning)

- go into EC2
  -> Lanuch Configuration -> select AMI -> create lanuch configuration -> create auto scailing group

- In Advanced Details,
     - check off "Receive traffic fron one or more load balancers.
     - add Classic Load Balancers
     - Health check -> ELB
- Select "Use scaling policies to adjust the capacity of this group and specify the minimum and maximum size of yout group.
     - then you can create alarm like when CPU utilization be below a threshold, then add ec2 instances
     - same you can set when to drop the ec2 instances

## Route 53

Route 53 is where you configure and manage web domain for websites or applications that you host on AWS.
Basically when you think about Route 53 you think about the domain (registration, DNS).

- So you can use Route 53 for domain registration like "example.com"

- It help translate domain name into IP address like 192.0.2.1, using DNS Server internally.

- Is also performs health check your domain.

![r53](/route53.png)

- you can register a domain in R53 or transfer a domain in R53 by paying the fee, currently \$12/year.
- and then create record sets, such as
     - www.{your_domain}
          - and select elb target

## Lambda

Lambda provides serverless computing. You can essentially imagine that it does all above mentioend (ec2 , auto scaling etc) and just managed by AWS already. 
It charges per number of calls and the amoubnt of ime(duration) it takes to execute the code.

##  hello world
- filter hello world
- configure trigger
- configure function
  - write your code here (something like below)
```python
def lambda_handler(event, context):
    print(event['key1']
    return event['key1']
```

## A full overview

This is a full overview of putting together all the elements mentioned above.

[final](/final.png)
